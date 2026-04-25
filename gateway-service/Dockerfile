
FROM node:20-slim


WORKDIR /app


COPY package*.json ./


RUN npm install express axios

COPY . .


EXPOSE 3003


CMD [ "node", "app.js" ]
