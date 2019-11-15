# EXEBlock Knowledge Base : How to create 5050dapp docker

1. Download the attached files
2. chmod +x build.sh
3. Open build.sh file and modify the blockchain\_url, faucet\_url, config\_name, host and container ports
4. Run the build.sh file

**Dockerfile**

FROM node:9

\# chain url  
ARG chain\_url  
ENV CHAIN\_URL=$chain\_url

\# faucet url  
ARG faucet\_url  
ENV FAUCET\_URL=$faucet\_url

\# set the port  
ARG port  
ENV PORT=$port  
EXPOSE $PORT

\# set the config name  
ARG config\_name  
ENV CONFIG\_NAME=$config\_name

RUN apt-get update && \  
apt-get install nano && \  
cd ~ && \  
git clone -b development [https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/5050dapp.git](https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/5050dapp.git) /home/5050dapp

WORKDIR /home/5050dapp

ADD serve.sh ./

RUN chmod +x serve.sh && \  
npm install && \  
sed -i "s\|\"scripts\": {\|\"scripts\": {\n\t\"build-$CONFIG\_NAME\": \"export NODE\_ENV=development\&\&export NODE\_CONFIG=dev.$CONFIG\_NAME\&\&webpack --config webpack\/webpack.config.js\",\n\t\"start-$CONFIG\_NAME\": \"serve ./dist -s -p $PORT\",\|" package.json && \  
cd webpack && \  
echo "{\n\t\"BLOCKCHAIN\_URL\": \"$CHAIN\_URL\",\n\t\"FAUCET\_URLS\": \[\n\t\t\"$FAUCET\_URL\"\n\t\]\n}" &gt; config.dev.$CONFIG\_NAME.json && \  
cd .. && \  
npm run-script build-$CONFIG\_NAME

  
CMD \["/bin/bash", "serve.sh"\]

**build.sh**

\# read the chain id  
CHAIN\_URL=[ws://10.20.10.49:8090/ws](exeblock-knowledge-base-how-to-create-5050dapp-docker.md)  
FAUCET\_URL=[http://10.20.10.49:3000/api/v1/accounts](http://10.20.10.49:3000/api/v1/accounts)  
HOST\_PORT=80  
CONTAINER\_PORT=8081  
CONFIG\_NAME=49

\# build 5050dapp  
docker build -t exeblock/5050dapp --build-arg chain\_url=$CHAIN\_URL --build-arg faucet\_url=$FAUCET\_URL --build-arg port=$CONTAINER\_PORT --build-arg config\_name=$CONFIG\_NAME --no-cache .

docker run -dit --name 5050dapp -p $HOST\_PORT:$CONTAINER\_PORT exeblock/5050dapp

**serve.sh**

nohup npm run-script start-$CONFIG\_NAME &

/bin/bash

