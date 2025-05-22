# Build stage
FROM node:20-alpine AS build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production-only image
FROM node:20-alpine AS prod
WORKDIR /app

# Install Doppler CLI
RUN apk add --no-cache wget \
    && wget -q -t3 'https://packages.doppler.com/public/cli/rsa.8004D9FF50437357.key' -O /etc/apk/keys/cli@doppler-8004D9FF50437357.rsa.pub \
    && echo 'https://packages.doppler.com/public/cli/alpine/any-version/main' | tee -a /etc/apk/repositories \
    && apk add doppler

COPY package.json package-lock.json ./
RUN npm ci --only=production

# Copy compiled output from build stage
COPY --from=build /app/dist ./dist
COPY --from=build /app/package.json ./

EXPOSE 3000

# Add Docker Healthcheck
HEALTHCHECK CMD wget --no-verbose --tries=1 --spider http://localhost:3000 || exit 1

# Start using Doppler secrets injection
CMD ["doppler", "run", "--", "node", "dist/index.js"]
