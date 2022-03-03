# FROM node:latest

# WORKDIR /usr/src/app

# COPY package*.json ./

# RUN npm install -g npm@7.20.6

# COPY /app /usr/src/app/

# EXPOSE 3000

# CMD ["node", "app.js"]

FROM node:latest as APP

WORKDIR /usr/src/app/

COPY app /usr/src/app/

RUN npm install

FROM node:alpine

COPY --from=APP /usr/src/app/ /usr/src/app

WORKDIR /usr/src/app/

EXPOSE 3000

CMD node seeds/seed.js && npm start