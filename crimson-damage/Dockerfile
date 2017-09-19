FROM node:6.9.1-alpine
RUN apk update && apk upgrade
RUN apk add git curl openssh
RUN rm -rf /var/cache/apk/*
RUN git clone https://github.com/ewolff/crimson-damage.git
RUN cd crimson-damage && npm install
RUN cd crimson-damage && npm run compile
CMD cd crimson-damage && npm run start
EXPOSE 3003
