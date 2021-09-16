FROM node:14.17.6 as base

WORKDIR /code

COPY package.json /code/package.json
COPY package-lock.json /code/package-lock.json
#
# BUILDER
#
FROM node:12.18.4 as build

WORKDIR /code

COPY package.json /code/package.json
COPY package-lock.json /code/package-lock.json

RUN npm ci --production

COPY . .
RUN npm run build

#
# DEV
#
FROM base as dev

RUN npm install

COPY . .
RUN npm run start

#
# PROD
#
FROM nginx:1.12-alpine as prod

COPY --from=build /code/build /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]