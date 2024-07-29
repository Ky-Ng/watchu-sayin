FROM node:20-alpine AS base

FROM base AS deps
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm ci

FROM base AS builder
WORKDIR /usr/src/app
COPY --from=deps /usr/src/app/node_modules ./node_modules
COPY . .
RUN npm run build

FROM base AS runner
WORKDIR /usr/src/app
ENV NODE_ENV production
COPY --from=builder /usr/src/app/public ./public
COPY --from=builder /usr/src/app/.next ./.next
COPY --from=builder /usr/src/app/node_modules ./node_modules
COPY --from=builder /usr/src/app/package.json ./package.json

EXPOSE 8081
ENV PORT 8081

CMD ["npm", "start"]
