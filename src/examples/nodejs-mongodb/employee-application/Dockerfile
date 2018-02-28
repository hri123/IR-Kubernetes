FROM ubuntu:16.04

RUN apt-get update && apt-get -y upgrade && apt-get install -y curl sudo
RUN curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash - \
&& apt-get install -y nodejs

ENV MY_MONGO_SERVICE_HOST=mongo
ENV MY_MONGO_SERVICE_PORT=27017

WORKDIR /opt/sample/Employee/
COPY opt/sample/Employee/ .

RUN npm install

CMD [ "npm", "start" ]
