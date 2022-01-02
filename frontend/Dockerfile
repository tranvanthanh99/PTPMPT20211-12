FROM node:lts-alpine AS package

WORKDIR /app

COPY package.json .

ENV NODE_ENV=production

RUN yarn

FROM node:lts AS build

WORKDIR /app

COPY --from=package /app/node_modules ./node_modules

COPY . .

RUN yarn build

FROM nginx:latest

RUN rm /etc/nginx/conf.d/default.conf

COPY --from=build /app/build /www/html

COPY nginx /etc/nginx/conf.d