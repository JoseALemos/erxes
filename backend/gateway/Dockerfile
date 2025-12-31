FROM node:22-alpine AS base
WORKDIR /app
RUN npm install -g pnpm

# Copiar archivos de configuración del monorepo
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml tsconfig.base.json nx.json ./

# Copiar los packages necesarios
COPY backend/erxes-api-shared ./backend/erxes-api-shared
COPY backend/gateway ./backend/gateway

# Instalar dependencias
RUN pnpm install --frozen-lockfile

# Build erxes-api-shared primero
RUN pnpm --filter erxes-api-shared build || true

# Build gateway
RUN pnpm --filter gateway build

# Runtime
FROM node:22-alpine
WORKDIR /app

RUN npm install -g pnpm

COPY --from=base /app ./

WORKDIR /app/backend/gateway

EXPOSE 4000

CMD ["pnpm", "start"]
