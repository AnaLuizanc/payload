# Backend Dockerfile for Next.js + Payload
FROM node:18-alpine AS build
WORKDIR /app

# Copy package files and install dependencies
COPY package.json package-lock.json* ./
RUN npm install

# Copy source and build
COPY . .
RUN npm run build

# Production image
FROM node:18-alpine
WORKDIR /app

# Copy built application
COPY --from=build /app/.next ./.next
COPY --from=build /app/node_modules ./node_modules
COPY --from=build /app/package.json ./package.json
COPY --from=build /app/src ./src

# Copy script de inicialização diretamente (não da build stage)
COPY init.sh ./

# Copy database and env files
COPY Payload.db* ./
COPY .env* ./

# Tornar o script executável e corrigir line endings
RUN chmod +x ./init.sh && \
    sed -i 's/\r$//' ./init.sh

EXPOSE 3000
CMD ["sh", "./init.sh"]
