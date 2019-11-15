# EXEBlock Knowledge Base : How to create PBSA faucet \(tapin\) docker container

\*\*NOTE: See attached Dockerfile

\*\*NOTE: The step of passing and updating chain id in the config.yml file is not ready yet, and therefore, has to be done manually. 

INSTRUCTIONS:

1. Create a new file and call it Dockerfile
2. Copy the content below into the file and save
3. Copy attached _build.sh_, build\_run.sh and _start\_tapin.sh_ scripts to the same directory
4. Navigate to the directory
5. Make the build scripts executable: _chmod +x ./build.sh  chmod +x ./buid\_run.sh_
6. Run the _./build.sh_    \(Optional\) You can also run the _./build\_run.sh_ to build the image and run the container __
7.  When completed, run the docker: _docker run -it --name tapin -p 3000:3000  -e FAUCET\_PORT =3000 exeblock/tapin_
8. In the container: make sure the faucet is running, and the port is correct
9. Type 'Exit'
10. The container will stop, so to run it type: _docker container start tapin_ 
11. \(To debug and test\) Connect with a separate console: _docker exec -it tapin /bin/bash_

FROM alpine:latest

\# set the faucet port  
ARG faucet\_port  
ENV FAUCET\_PORT=$faucet\_port  
EXPOSE $FAUCET\_PORT

\# chain id  
ARG chain\_id  
ENV CHAIN\_ID=$chain\_id

\# witness url  
ARG witness\_url  
ENV WITNESS\_URL=$witness\_url

\# Home folder  
ARG HOME\_DIR=/home/faucet

\# exeblock-peerplays-python folder  
ARG PEERPLAYS\_LIB\_DIR=$HOME\_DIR/exeblock-peerplays-python

\# tapin folder  
ARG TAPIN\_DIR=$HOME\_DIR/tapin

\# Update all the packages of a running system, and install dependencies  
RUN apk add --no-cache python3 && \  
apk add python3-dev build-base --update-cache && \  
python3 -m ensurepip && \  
rm -r /usr/lib/python\*/ensurepip && \  
pip3 install --upgrade pip setuptools && \  
if \[ ! -e /usr/bin/pip \]; then ln -s pip3 /usr/bin/pip ; fi && \  
if \[\[ ! -e /usr/bin/python \]\]; then ln -sf /usr/bin/python3 /usr/bin/python; fi && \  
rm -r /root/.cache && \  
apk add --no-cache gcc && \  
apk add gcc g++ make libffi-dev openssl-dev && \  
apk add bash && \  
apk add git

\# Create 'faucet' folder  
RUN mkdir $HOME\_DIR

WORKDIR $HOME\_DIR

\# Clone exeblock-peerplays-python  
RUN git clone [https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/exeblock-peerplays-python.git](https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/exeblock-peerplays-python.git)

\#Build exeblock-peerplays-python  
WORKDIR $PEERPLAYS\_LIB\_DIR  
RUN python3 -m pip install . --no-cache-dir

\# Downgrade graphene-lib to 0.6.0  
WORKDIR $PEERPLAYS\_LIB\_DIR  
RUN python3 -m pip uninstall -y graphenelib && \  
python3 -m pip install -Iv [https://files.pythonhosted.org/packages/2f/10/5624a93edfe72e703beb38e92892a0730394f77761c68a52255a361ef663/graphenelib-0.6.0.tar.gz](https://files.pythonhosted.org/packages/2f/10/5624a93edfe72e703beb38e92892a0730394f77761c68a52255a361ef663/graphenelib-0.6.0.tar.gz) --no-cache-dir

\# Install tapin  
WORKDIR $HOME\_DIR  
RUN git clone [https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/tapin.git](https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/tapin.git)

WORKDIR $TAPIN\_DIR  
\# Install dependencies  
RUN python3 -m pip uninstall -y pycryptodome && \  
python3 -m pip install pycrypto && \  
\#python3 -m pip uninstall -y pycryptodome && \  
\#python3 -m pip install pycryptodomex && \  
python3 -m pip install pyyaml && \  
python3 -m pip install flask && \  
python3 -m pip install flask-SQLAlchemy && \  
python3 -m pip install flask-Session && \  
python3 -m pip install flask-cors && \  
python3 -m pip install flask-script && \  
python3 -m pip install flask-mail && \  
python3 -m pip install gunicorn && \  
python3 -m pip install configparser && \  
python3 -m pip install requests

\# Prepare a new copy of the config file  
WORKDIR $TAPIN\_DIR  
RUN cp config-example.yml config.yml

WORKDIR $TAPIN\_DIR  
\# Update the 'witness\_url' in the config.yml file  
RUN sed -i "14s\|.\*\|witness\_url: \"${witness\_url}\"\|" config.yml  
\# Update the 'chain\_id' in the config.yml file  
RUN sed -i "29s\|.\*\|chain\_id: \"${chain\_id}\",\|" config.yml  
\# Set the default PPY balance to 100,000  
RUN sed -i "34s\|.\*\|initial\_balance: 100000\|" config.yml

\#Run configuration script \(from the root of the faucet\)  
WORKDIR $TAPIN\_DIR  
RUN python3 manage.py install

ADD start\_tapin.sh $TAPIN\_DIR  
WORKDIR $TAPIN\_DIR  
RUN chmod +x ./start\_tapin.sh

\#Run the faucet  
WORKDIR $TAPIN\_DIR  
CMD \["/bin/bash", "./start\_tapin.sh"\]

\#WORKDIR $TAPIN\_DIR  
\#ENTRYPOINT python3 manage.py runserver -h 0.0.0.0 -p $FAUCET\_PORT &

