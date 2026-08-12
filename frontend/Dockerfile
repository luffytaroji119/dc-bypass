# Frontend — TanStack Start (Nitro node-server) for Railway
FROM node:20-slim AS deps
WORKDIR /app
COPY package.json bun.lock* package-lock.json* ./
RUN npm install --no-audit --no-fund

FROM node:20-slim AS build
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
# Build as a standalone Node server (not Cloudflare) so it runs on Railway
ARG NITRO_PRESET=node-server
ENV NITRO_PRESET=$NITRO_PRESET
ARG VITE_API_BASE
ENV VITE_API_BASE=$VITE_API_BASE
RUN npm run build

FROM node:20-slim AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=build /app/.output ./.output
EXPOSE 3000
CMD ["node", ".output/server/index.mjs"]
