FROM node:22.14.0

# 一時的にルートユーザーを使用
USER root

RUN npm install -g npm@latest vercel@latest

RUN npx playwright install-deps
RUN npx playwright install

# 非ルートユーザーに切り替え
USER node

# ディレクトリの指定
WORKDIR /home/node/app