FROM node:6.9.1-alpine
RUN apk update && apk upgrade
RUN apk add git curl openssh
RUN rm -rf /var/cache/apk/*
RUN git clone https://github.com/ewolff/crimson-backend.git
RUN cd crimson-backend && npm install
CMD cd crimson-backend && npm run start
EXPOSE 8080
