FROM java:8-jre

RUN apt-get update -q
RUN apt-get install -y python-pip
RUN pip install --upgrade --user awscli
ENV PATH ~./local/bin:$PATH

RUN apt-get install -y git
RUN git clone -o github https://github.com/promptworks/aws-secrets ; cd aws-secrets ; make install
