FROM node:24.22.1-bookworm-slim AS build
ARG NODE_ENV
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 \
    make \
    g++ \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci --include=dev

COPY angular.json tsconfig.json ./
COPY src ./src

RUN ng build

FROM node:24.22.1-bookworm-slim AS run

WORKDIR /app

COPY --from=build /app/dist ./dist

EXPOSE 4200

USER node

CMD ["node", "dist/server/server.mjs"]
