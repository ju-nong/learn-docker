FROM node:18.16.0 as build

WORKDIR /app

COPY public ./public
COPY src ./src
COPY index.html .
COPY package-lock.json .
COPY package.json .
COPY tsconfig.json .
COPY tsconfig.node.json .
COPY vite.config.ts .

RUN npm install --silent
RUN npm run build

FROM nginx

COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist /usr/share/nginx/html

ENTRYPOINT ["nginx", "-g", "daemon off;"]