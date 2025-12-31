FROM node:22-alpine AS base
WORKDIR /app
RUN npm install -g pnpm

# Copiar archivos de configuración del monorepo
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml tsconfig.base.json nx.json ./

# Copiar los packages necesarios
COPY backend/erxes-api-shared ./backend/erxes-api-shared
COPY backend/core-api ./backend/core-api

# Instalar dependencias
RUN pnpm install --frozen-lockfile

# Build
RUN pnpm --filter core-api build

# Runtime
FROM node:22-alpine
WORKDIR /app

RUN npm install -g pnpm

COPY --from=base /app ./

WORKDIR /app/backend/core-api

EXPOSE 3300

CMD ["pnpm", "start"]
