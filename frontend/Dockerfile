# ------------------------
# 1. Build Stage
# ------------------------
FROM node:18-alpine AS builder
WORKDIR /app

# Copy root package.json to the container
COPY package.json package-lock.json ./

# Install root-level dependencies
RUN npm install

# Copy ONLY frontend source
COPY frontend ./frontend

# Move into frontend directory
WORKDIR /app/frontend

# Install frontend-specific dependencies
RUN npm install

# Build Next.js app
RUN npm run build

# ------------------------
# 2. Production Stage
# ------------------------
FROM node:18-alpine AS runner
WORKDIR /app

ENV NODE_ENV=production
ENV PORT=3000

# Copy build output
COPY --from=builder /app/frontend/.next ./.next
COPY --from=builder /app/frontend/public ./public
COPY --from=builder /app/frontend/package.json ./
COPY --from=builder /app/frontend/node_modules ./node_modules

EXPOSE 3000
CMD ["npm", "start"]
