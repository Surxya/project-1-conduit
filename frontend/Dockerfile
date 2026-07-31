# Stage 1: Build React App
FROM node:18-alpine AS builder

WORKDIR /app

# Copy package manifests & install dependencies
COPY package*.json ./
RUN npm install

# Copy source code and build production bundle
COPY . .
RUN npm run build

# Stage 2: Serve with Nginx
FROM nginx:alpine AS runner

# Copy built static files from builder to Nginx web root
COPY --from=builder /app/build /usr/share/nginx/html

# Expose HTTP port
EXPOSE 80

# Run Nginx in foreground
CMD ["nginx", "-g", "daemon off;"]
