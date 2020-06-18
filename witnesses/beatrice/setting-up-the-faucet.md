# Setting up the faucet

It is recommended to use Ubuntu 18.04 to run the faucet.

## Installation

### Clone the Repository

```text
git clone https://gitlab.com/PBSA/PeerplaysIO/tools-libs/faucet.git -b develop
```

### Install the dependencies

```text
sudo apt update
sudo apt-get install libffi-dev libssl-dev python-dev python3-pip redis-server 
```

{% hint style="info" %}
It is best practice to use python virtual environments. [https://docs.python.org/3/tutorial/venv.html](https://docs.python.org/3/tutorial/venv.html)
{% endhint %}

```text
python3 -m venv env
source env/bin/activate
```

```text
pip3 install -r requirements.txt
```

### Edit the configuration

```text
cp config-example.yml config.yml
```

Change the `config.yml` for Beatrice and your node:

```text
secret_key: "RANDOM-STRING"  # Random keys
nobroadcast: True            # Safety mode

mail_host: "SERVER:589"
mail_user: "user"
mail_pass: "password"
mail_from: "noreply@faucet.org"

admins:
 - adminA@example.com
 - adminB@example.com

minIPAge: 300  # How long in secs does an IP need to wait to register a new account?
witness_url: "<your node endpoint>"

registrar: "<registrar account name>"
default_referrer: "<referrer account name>"
referrer_percent: 50  # in percent
wif: "<WIF for your account>"

balance_mailthreshold: 500  # if balances goes below this, you will be notified
core_asset: "TEST"
```

### Apply the changes

```text
python3 manage.py install
```

## Usage

```text
python work.py # for starting the worker

python manage.py run # for normal mode

python manage.py start # for debug mode
```

The faucet will then be available at `http://localhost:5000`

#### Testing

Once the faucet is running, tests can be run.

```text
python3 test_faucet.py
```

## Production Configuration

### NGINX

Run uwsgi

```text
uwsgi --ini wsgi.ini
```

and use a configuration similar to this:

```text
user peerplays;
worker_processes  4;

events {
    worker_connections  2048;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    access_log  /www/logs/access.log;
    error_log  /www/logs/error.log;
    log_not_found off;
    sendfile        on;
    keepalive_timeout  65;
    gzip  on;

    upstream websockets {
      server localhost:9090;
      server localhost:9091;
    }

    server {
        listen       80;
        if ($scheme != "https") {
                return 301 https://$host$request_uri;
        }

        listen       443 ssl;
        server_name  peerplays-wallet.com;
        ssl_certificate      /etc/nginx/ssl/peerplays-wallet.com.crt;
        ssl_certificate_key /etc/nginx/ssl/peerplays-wallet.com.key;
        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;
        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;

        location ~ /ws/? {
            access_log on;
            proxy_pass http://websockets;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header Host $host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_next_upstream     error timeout invalid_header http_500;
            proxy_connect_timeout   2;
        }
        location ~ ^/[\w\d\.-]+\.(js|css|dat|png|json)$ {
            root /www/wallet;
            try_files $uri /wallet$uri =404;
        }
        location / {
            root /www/wallet;
        }
        location /api {
                include uwsgi_params;
                uwsgi_pass unix:/tmp/faucet.sock;
        }

    }
}
```

