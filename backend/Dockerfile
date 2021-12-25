FROM node:lts-alpine
ENV NODE_ENV=production
WORKDIR /usr/src/app
COPY ["package.json", "package-lock.json*", "npm-shrinkwrap.json*", "./"]
RUN npm install --production --silent && mv node_modules ../
COPY . .
ENV PORT=5000
ENV DB_NAME=PROJECTX
ENV DB_HOST=admin
ENV DB_KEY=QcVC12L5pDXhieTK
ENV DB_PORT=@cluster0.tizlc.mongodb.net
ENV JWT_SECRET="PROJECTX_SECRETKEY"
EXPOSE 5000
RUN chown -R node /usr/src/app
USER node
CMD npm start
