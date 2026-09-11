FROM node:24.21.0-alpine3.24 as build
WORKDIR /nsse-frontend
COPY package*.json ./
RUN npm install --global npm@10.9.2 && npm ci --silent --only=production
COPY . .
RUN npm run build

FROM nginxinc/nginx-unprivileged:1.30.4-alpine3.24
COPY --from=build /nsse-frontend/dist /usr/share/nginx/html
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx/nginx.conf /etc/nginx/conf.d/nginx.conf
USER nginx
HEALTHCHECK CMD curl --fail http://localhost:3000/ || exit 1

EXPOSE 3000
ENTRYPOINT ["nginx", "-g", "daemon off;"]