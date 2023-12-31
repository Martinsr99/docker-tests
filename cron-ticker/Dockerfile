# FROM --platform=$BUILDPLATFORM node:19.2-alpine3.16
# docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7,linux/arm64/v8 -t martinsr99/cron-ticker --push .
FROM node:19.2-alpine3.16 as deps
# cd app
WORKDIR /app
# Dest /app = ./
COPY package.json ./
# Install dependencies
RUN npm install

# Builder and tests
FROM node:19.2-alpine3.16 as builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run test

# PROD dependencies
FROM node:19.2-alpine3.16 as prod-deps
WORKDIR /app
COPY package.json ./
RUN npm install --prod

# Runner
FROM node:19.2-alpine3.16 as runner
WORKDIR /app
COPY --from=prod-deps /app/node_modules ./node_modules
# Dest /app = ./
COPY app.js ./
COPY tasks/ ./tasks
# Run the image
CMD ["node", "app.js"]