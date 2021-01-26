# Connecting Elasticsearch to a blockchain node

### Editing the configuration for Elasticsearch <a id="3.-Editing-the-configuration-for-Elasticsearch"></a>

Inside ./witness\_node\_data\_dir/config.ini edit the following:

Uncomment `plugins =` and add the `elasticsearch` and `es_object` plugins.

```text
# Space-separated list of plugins to activate 
plugins = elasticsearch es_objects
```

Uncomment `elasticsearch-node-url =` and add the Endpoint URL for your Elasticsearch instance.

Make sure to keep the trailing slash at the end of the URL.

```text
# Elastic Search database node url(http://localhost:9200/) 
elasticsearch-node-url = <ES-ENDPOINT>
```

Uncomment `es-objects-elasticsearch-url =` and add the Endpoint URL for your Elasticsearch instance.

Make sure to keep the trailing slash at the end of the URL.

```text
# Elasticsearch node url(http://localhost:9200/) 
es-objects-elasticsearch-url = <ES-ENDPOINT>
```

### Start the witness node <a id="4.-Start-the-witness-node"></a>

Start the witness node to being pushing indexes to Elasticsearch. The beginning few logs should show the Elasticsearch plugins started:

```text
572387ms th_a elasticsearch_plugin.cpp:516 plugin_startup ] elasticsearch ACCOUNT HISTORY: plugin_startup() begin
572387ms th_a application.cpp:1235 startup_plugins ] Plugin elasticsearch started
572395ms th_a es_objects.cpp:405 plugin_startup ] elasticsearch OBJECTS: plugin_startup() begin
572395ms th_a application.cpp:1235 startup_plugins ] Plugin es_objects started ...

575659ms th_a es_objects.cpp:82 genesis
```

## Basic Checks <a id="Basic-Checks"></a>

You can check the indexes created after the witness start with the next call:

```text
$ curl -X GET '<ES-ENDPOINT>/_cat/indices'
yellow open ppobjects-asset   vnF2F0BMSKKNxQfu013OCQ 1 1  2 0 12.4kb 12.4kb
yellow open ppobjects-balance 3psSvHIeRC-fKHyqP2Legg 1 1  1 0  5.9kb  5.9kb
yellow open ppobjects-account zQ7uv01ESEeqCED3WHmmxg 1 1 21 0 42.4kb 42.4kb
```

![](.gitbook/assets/image%20%2812%29.png)

You can also get index search data with:

```text
curl -X GET '<ES-ENDPOINT>/peerplays-*/data/_search?pretty=true' -H 'Content-Type: application/json' 
```

