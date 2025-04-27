# 開発環境
FROM node:22.14.0-alpine AS dev

ENV NEXT_TELEMETRY_DISABLED 1
WORKDIR /home/node/app

# 非ルートユーザーに切り替え
USER node

COPY --chown=node:node ./app /home/node/app

RUN npm install

# ビルド
FROM dev AS build

# 
WORKDIR /home/node/app

# 非ルートユーザーに切り替え
USER node

RUN npm install

ENV NODE_ENV production

# コードのビルド
RUN npm run build

# 本番環境
FROM gcr.io/distroless/nodejs22-debian12 AS production

COPY --from=build /home/node/app/next.config.ts ./
COPY --from=build /home/node/app/public ./public
COPY --from=build /home/node/app/.next ./.next
COPY --from=build /home/node/app/node_modules ./node_modules

ENV PORT 3000
ENV NEXT_TELEMETRY_DISABLED 1

WORKDIR /
CMD ["./node_modules/next/dist/bin/next", "start"]