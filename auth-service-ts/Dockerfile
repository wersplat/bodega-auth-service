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
RUN apk add --no-cache --repository https://packages.doppler.com/alpine/v3.18 main doppler

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
