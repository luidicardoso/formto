FROM node:20.18-alpine AS builder

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .

# Vazio = URLs relativas (o nginx.conf local faz o proxy reverso)
ARG VITE_API_BASE_URL=
ENV VITE_API_BASE_URL=$VITE_API_BASE_URL

RUN npm run build

# Serve with nginx
FROM nginx:1.27-alpine

COPY --from=builder /app/dist /usr/share/nginx/html

# Copia o nginx.conf criado para a pasta de configuração do Nginx
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
