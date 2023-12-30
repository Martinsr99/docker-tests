# FROM --platform=$BUILDPLATFORM node:19.2-alpine3.16
# docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7,linux/arm64/v8 -t martinsr99/cron-ticker --push .
FROM node:19.2-alpine3.16

# cd app
WORKDIR /app

# Dest /app = ./
COPY package.json ./

# Install dependencies
RUN npm install

# Dest /app = ./
COPY . .

# Testing
RUN npm run test

# Remove unnecessary files for PROD
RUN rm -rf tests && rm -rf node_modules

# Install only for PROD
RUN npm install --prod

# Run the image
CMD ["node", "app.js"]