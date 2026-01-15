# ========================================
# Stage 1: Builder
# ========================================
FROM node:20-alpine AS builder

WORKDIR /app

# Copy package files for layer caching
COPY package.json package-lock.json* ./

# Install ONLY production dependencies
RUN npm install --only=production --omit=dev

# ========================================
# Stage 2: Production (Distroless)
# ========================================
FROM gcr.io/distroless/nodejs20-debian12:nonroot

WORKDIR /app

# Copy dependencies from builder
COPY --from=builder /app/node_modules ./node_modules

# Copy application code
COPY . .

# Expose port (adjust if your app uses different port)
EXPOSE 3000

# CMD not ENTRYPOINT - distroless has 'node' as default entrypoint
# This runs: node server.js
CMD ["server.js"]
