FROM java:8-jre

RUN apt-get update -q
RUN apt-get install -y python-pip

RUN apt-get install -y build-essential libssl-dev libffi-dev python-dev
RUN pip install --upgrade cffi credstash
