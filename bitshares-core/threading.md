# Threading

_Work in progress_

## General Remarks

BitShares uses `boost::thread` for parallel execution. However, due to the single-threaded internal database, threads are used sparingly.

On top of `boost::thread`, BitShares implements a kind of cooperative multitasking \(using `boost::context`\). I. e. a thread can have tasks queued for processing, and whenever a task either completes, is blocked in a mutex, or is waiting for a future, the thread switches to the next task in the queue \(but see below\).

Per default there are two main threads, a database thread and a P2P thread. In addition, several more threads are started for asynchronous IO operations \(using `boost::asio`\). These are available through `fc::default_io_service()`.

With [https://github.com/bitshares/bitshares-core/pull/1360](https://github.com/bitshares/bitshares-core/pull/1360) there will be another thread pool. It is mostly meant for parallel processing of computationally intensive things that do not otherwise interfere with each other. This pool is accessed using `fc::do_parallel()`.

## P2P Thread

The P2P thread is "owned" by `node_impl`. It has several permanent tasks for handling \(accepting, creating, monitoring, closing\) P2P connections, started from `connect_to_p2p_network()`.

P2P communication is based on messages. Messages start with a message header containing message type and payload size, followed by the payload and padded to a multiple of 16 bytes. Incoming messages concerning the P2P layer are handled within the P2P thread, messages concerning the database \(mostly blocks and transactions\) are handed over to the database thread. "handed over" means a task is created within the database thread.

### accept\_loop

Accepts connections on the P2P listening socket \(if any\) and enters them into the `_handshaking_connections` pool.

### p2p\_network\_connect\_loop

Attempts to connect to known peers as long as `current_connections < desired_connections`.

### fetch\_sync\_items\_loop

Continually requests items in `ids_of_items_to_get` from connected peers.

### fetch\_items\_loop

Continually requests items in `_items_to_fetch` from connected peers.

### advertise\_inventory\_loop

Advertises items in `_new_inventory` to connected in-sync peers.

### terminate\_inactive\_connections\_loop

Terminates connections that appear idle, or stale, or dead.

### fetch\_updated\_peer\_lists\_loop

Periodically \(hardcoded every 15 minutes\) sends `address_request_message` to all active peers.

### bandwidth\_monitor\_loop

Updates total bytes sent/received every second for the past minute. Aggregates per minute over the last hour, keeps hourly aggregates for 72 hours.

### dump\_node\_status\_task

Dumps node status to logfile \(level INFO\) every minute.

## Database thread

The database thread is actually the "main" thread of `witness_node`.

It initializes the database and the configured plugins, opens the database \(which can involve replaying\), then starts the P2P network thread and possibly websocket API endpoints. After starting everything else, it simply goes to sleep and waits for application shutdown.

The database thread does not remain completely inactive though. It handles tasks posted to it from the other threads, for example from the P2P thread, from the websocket threads, or from plugins like the witness plugin.

Some of these tasks **must not** be interrupted. E. g. when during application of a block the task yields, another task may become active that interferes with the block processing, e. g. when a new pending transaction is received via P2P or client API and immediately applied to the database.

## Tasks

An `fc::task` consists of two parts - a functor to be executed, and a promise that is fulfilled upon completion of the functor with its return value.

An `fc::promise` is usually paired with an `fc::future`. The future owns the promise \(in the sense of RAII\) through a `shared_ptr`. This means that at least one future for a given task must exist until the task is completed, because otherwise the promise goes out of scope and the thread that's processing the task will have a problem.

## **Caveats**

* A thread that is blocked in an `fc::spin_yield_lock` will yield to other tasks in its ready queue, but will not start new tasks in the pending queue, nor check blocked tasks and the futures they're blocking on!
* A thread that is blocked in an `fc::spin_lock` will \(as the name implies\) spin-wait for the lock to become free instead of yielding to anything else.
* `promise.wait(timeout)` will set the promise into error state if the timer expires.

