# this image is uses a multi-stage build 

# first build a node image that is used to install our deps and build our final static files
FROM node:16-alpine as builder
WORKDIR '/app'
COPY ./package.json ./
RUN npm install
COPY . .
RUN npm run build

# create an nginx image that serves the build folder from the previous stage
FROM nginx
EXPOSE 3000
# copy in the nginx config file which is used to determine how nginx should be configured
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
# copy build directory into the html directory that nginx servers its static files from
COPY --from=builder /app/build /usr/share/nginx/html