# ---------- Build Stage ----------
FROM node:22.20.0-alpine3.22 AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

# ---------- Runtime Stage ----------
FROM nginx:1.29.1-alpine3.22

COPY --from=builder /app/dist /usr/share/nginx/html

COPY ../nginx/nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]