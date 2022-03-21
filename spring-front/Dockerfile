FROM node:16.14.2-alpine as builder
WORKDIR /app
ENV PATH /app/node_modules/.bin:$PATH
COPY package.json ./
COPY package-lock.json ./
RUN npm ci
RUN npm install react-scripts -g
COPY . ./
RUN npm run build

FROM node:lts-alpine3.13 as certbuilder
RUN apk add openssl
WORKDIR /certs
RUN openssl req -nodes -new -x509 -subj="/C=US/ST=Denial/L=springfield/O=Dis/CN=localhost" -keyout server.key -out server.cert

FROM nginx:stable-alpine
COPY --from=builder /app/build /usr/share/nginx/html
COPY --from=certbuilder /certs /certs
CMD ["nginx", "-g", "daemon off;"]