###################################
#
#   開発環境
#
###################################

FROM node:22.14.0-slim AS dev

# 一時的にルートユーザーを使用
USER root

# グローバルインストール
RUN npm install -g npm@latest vercel@latest

# playwrightのインストール
RUN npx playwright install-deps
RUN npx playwright install

# 非ルートユーザーに切り替え
USER node

# ディレクトリの指定
WORKDIR /home/node/app

###################################
#
#   ビルド環境
#
###################################

FROM dev AS build

# 非ルートユーザーに切り替え
USER node

# ディレクトリの指定
WORKDIR /home/node/app

COPY --chown=node:node ./app/package*.json /home/node/app/
COPY --chown=node:node ./app /home/node/app/

RUN npm ci

# ビルドを実行
RUN npm run build

# 不必要なパッケージを削除
RUN npm prune --production

###################################
#
#   本番環境
#
###################################

FROM gcr.io/distroless/nodejs22-debian12:nonroot AS production

# ディレクトリの指定
WORKDIR /app

# ビルドステージからファイルをコピー
COPY --from=build --chown=nonroot:nonroot /home/node/package*.json ./
COPY --from=build --chown=nonroot:nonroot /home/node/app/next.config.ts ./
COPY --from=build --chown=nonroot:nonroot /home/node/app/public ./public
COPY --from=build --chown=nonroot:nonroot /home/node/app/.next ./.next
COPY --from=build --chown=nonroot:nonroot /home/node/app/node_modules ./node_modules

# 環境変数を設定
ENV NODE_ENV production

# Next.jsアプリケーションを起動
CMD ["node_modules/.bin/next", "start"]