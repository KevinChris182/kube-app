# Local Development
FROM node:18-alpine AS development

# Dependencies required for development
RUN npm install -g @nestjs/cli

WORKDIR /usr/src/app
COPY --chown=node:node package.json yarn.lock ./
RUN yarn install --frozen-lockfile
COPY --chown=node:node . .
USER node

# Build for production
FROM node:18-alpine AS build

WORKDIR /usr/src/app
COPY --chown=node:node package.json yarn.lock ./
COPY --chown=node:node --from=development /usr/src/app/node_modules ./node_modules
COPY --chown=node:node . .
RUN yarn run build
ENV NODE_ENV production
RUN yarn install --frozen-lockfile --production=true
USER node

# Production
FROM node:18-alpine AS production

COPY --chown=node:node --from=build /usr/src/app/node_modules ./node_modules
COPY --chown=node:node --from=build /usr/src/app/dist ./dist
COPY --chown=node:node --from=build /usr/src/app/package.json ./
EXPOSE 3000
CMD ["yarn", "run", "start:prod"]
USER node
