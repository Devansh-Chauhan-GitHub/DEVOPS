FROM node:18 AS base

WORKDIR /app

COPY /package*.json ./

RUN npm ci

COPY . .

#stage 2

FROM node:18-slim

WORKDIR /app

COPY --from=base /app /app

EXPOSE 4200

RUN npm install -g @angular/cli@13

CMD ["ng","serve","--host","0.0.0.0"]
