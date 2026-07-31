# Stage 1: Build Stage
FROM node:18-alpine AS builder

WORKDIR /app

# Install OpenSSL for Prisma engine compatibility
RUN apk add --no-cache openssl

# Copy package files and install dependencies
COPY package*.json ./
RUN npm ci

# Copy full source code
COPY . .

# Generate Prisma client and build the TypeScript project
RUN npx prisma generate
RUN npm run build

# Stage 2: Production Runtime
FROM node:18-alpine AS runner

WORKDIR /app

# Install OpenSSL in runtime stage as well
RUN apk add --no-cache openssl

ENV NODE_ENV=production

# Copy node_modules, dist, and package files
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/package*.json ./

EXPOSE 3000

USER node

# Start the application
CMD ["node", "dist/api/main.js"]
