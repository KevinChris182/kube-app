# Production
FROM node:18-alpine

COPY --chown=node:node --from=build /usr/src/app/node_modules ./node_modules
COPY --chown=node:node --from=build /usr/src/app/dist ./dist
COPY --chown=node:node --from=build /usr/src/app/package.json ./
EXPOSE 3000
CMD ["npm", "run", "start:prod"]
HEALTHCHECK --interval=1m --timeout=3s --start-period=1m \
    CMD wget --spider --tries=1 --no-verbose http://localhost:3000/api/v1/health || exit 1
USER node
