# ---------- Build Stage ----------
FROM node:22.20.0-alpine3.22 AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

# ---------- Runtime Stage ----------
FROM nginx:1.29.1-alpine3.22

LABEL org.opencontainers.image.title="React Frontend"
LABEL org.opencontainers.image.vendor="Vinod Deshpande"

COPY --from=builder /app/dist /usr/share/nginx/html

COPY nginx/nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
CMD wget --spider -q http://localhost/ || exit 1

CMD ["nginx", "-g", "daemon off;"]