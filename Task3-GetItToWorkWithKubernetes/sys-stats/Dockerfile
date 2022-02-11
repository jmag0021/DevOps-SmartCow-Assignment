# Dockerfile for node application
# docker build . -t sys-stats-image
# docker run --rm --name sys-stats -p 3000:80 sys-stats-image
# Check result on http://localhost:3000

# build environment
# "as build" label since we are using multi-stage build dockerfiles
FROM node:13.12.0-alpine as build

WORKDIR /app

ENV PATH /app/node_modules/.bin:$PATH

COPY package.json ./
COPY package-lock.json ./

# Using npm ci instead of npm install sinc it is better practice for automated env
RUN npm ci --silent

# -g sets a global configuration option
RUN npm install -g --silent

COPY . ./

# Build with production dependencies
RUN npm run build --production

# production environment
FROM nginx:stable-alpine

# copies files from previous build
COPY --from=build /app/build /usr/share/nginx/html

EXPOSE 80

# daemon off directive to run in the foreground
CMD ["nginx", "-g", "daemon off;"]å