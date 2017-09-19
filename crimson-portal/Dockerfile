FROM node:6.9.1-alpine
RUN apk update && apk upgrade
RUN apk add git curl openssh
RUN rm -rf /var/cache/apk/*
RUN git clone https://github.com/ewolff/crimson-portal.git
RUN cd crimson-portal && npm install
RUN cd crimson-portal && npm run compile
CMD cd crimson-portal && npm start
EXPOSE 3000
