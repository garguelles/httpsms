# build stage
FROM node:lts-alpine as build

ARG FIREBASE_API_KEY
ARG FIREBASE_AUTH_DOMAIN
ARG FIREBASE_PROJECT_ID
ARG FIREBASE_STORAGE_BUCKET
ARG FIREBASE_MESSAGING_SENDER_ID
ARG FIREBASE_APP_ID
ARG FIREBASE_MEASUREMENT_ID

WORKDIR /app

COPY package.json pnpm-lock.yaml ./

# Install pnpm
RUN npm install -g pnpm

# install python
RUN apk add --no-cache python3

RUN pnpm install
COPY . .
RUN pnpm run generate

# production stage
FROM nginx:stable-alpine as production
COPY --from=build /app/dist /usr/share/nginx/html

# Copy the nginx configuration file
COPY ./nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 3000

CMD ["nginx", "-g", "daemon off;"]
