FROM node:8-alpine AS build

COPY . /app

WORKDIR  /app

RUN npm clean-install
RUN npm test

FROM node:8-alpine AS final

COPY . /app

WORKDIR  /app

EXPOSE 8080

RUN npm install

CMD npm start