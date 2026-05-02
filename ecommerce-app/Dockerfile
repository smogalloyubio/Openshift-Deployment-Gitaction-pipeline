
FROM node:18-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm install


COPY . .

RUN npm run build
FROM nginx:alpine-slim
RUN rm -rf /usr/share/nginx/html/* && rm  -rf /etc/nginx/conf.d/default.conf

COPY --from=build /app/build  /usr/share/nginx/html

COPY nginx.conf /etc/nginx/nginx.conf


RUN chmod -R 775 /var/cache/nginx /var/run /var/log/nginx /usr/share/nginx/html && \
    chgrp -R 0 /var/cache/nginx /var/run /var/log/nginx /usr/share/nginx/html

RUN sed -i 's/^user/#user/' /etc/nginx/nginx.conf

USER 1001

EXPOSE 8080


CMD ["nginx", "-g", "daemon off;"]
