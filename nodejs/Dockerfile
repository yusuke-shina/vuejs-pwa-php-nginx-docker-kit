FROM node:alpine

WORKDIR /app

COPY /data/html/app/package* ./

RUN npm install
CMD npm run serve