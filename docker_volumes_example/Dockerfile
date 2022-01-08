FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

# build time arguments, can use this on all instructions except for "RUN"
ARG DEFAULT_PORT=80

# default env variable
ENV PORT $DEFAULT_PORT

EXPOSE $PORT

CMD [ "npm", "start" ]