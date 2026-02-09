# Multi-stage build for the frontend
FROM node:20-alpine AS base

WORKDIR /app

# Install dependencies based on the preferred package manager
COPY package.json package-lock.json* ./
RUN npm ci --only=production

# Production build stage
FROM node:20-alpine AS builder

WORKDIR /app

# Install ALL dependencies (including dev dependencies) for building
COPY package.json package-lock.json* ./
RUN npm ci

# Copy application source code
COPY . .

# Build the application
RUN npm run build

# Production stage
FROM node:20-alpine AS production

WORKDIR /app

# Install serve to serve the Next.js app
RUN npm install -g serve

# Copy built application from builder stage
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./standalone
COPY --from=builder /app/.next/static ./.next/static

# Create non-root user for security
RUN addgroup -g 1001 -S nextjs && \
    adduser -S nextjs -u 1001

# Change ownership to non-root user
RUN chown -R nextjs:nextjs /app
USER nextjs

# Expose port (typically 3000 for Next.js)
EXPOSE 3000

# Serve the application
ENV NODE_ENV=production
CMD ["serve", "-s", "standalone", "-l", "3000"]