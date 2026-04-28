
FROM node:18-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm ci


COPY . .

RUN npm run build
FROM nginx:alpine-slim
RUN rm -rf /usr/share/nginx/html/*

COPY --from=build /app/dist /usr/share/nginx/html

COPY nginx.conf /etc/nginx/conf.d/default.conf


RUN chmod -R 775 /var/cache/nginx /var/run /var/log/nginx /usr/share/nginx/html && \
    chgrp -R 0 /var/cache/nginx /var/run /var/log/nginx /usr/share/nginx/html

RUN sed -i 's/^user/#user/' /etc/nginx/nginx.conf

USER 1001

EXPOSE 8080


CMD ["nginx", "-g", "daemon off;"]
