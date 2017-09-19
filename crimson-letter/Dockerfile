FROM node:6.9.1-alpine
RUN apk update && apk upgrade
RUN apk add git curl openssh
RUN rm -rf /var/cache/apk/*
RUN git clone https://github.com/ewolff/crimson-letter.git
RUN cd crimson-letter && npm install
RUN cd crimson-letter && npm run compile
CMD cd crimson-letter && npm run start
EXPOSE 3002
