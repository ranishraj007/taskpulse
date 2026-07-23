# STAGE 1: Build the React app
FROM node:20-alpine AS builder

WORKDIR /app

# Copy package manifests and lockfile
COPY package.json yarn.lock ./

# Install dependencies using Yarn
RUN yarn install --frozen-lockfile

# Copy source code and build
COPY . .
RUN yarn build


# STAGE 2: Serve compiled static files with Nginx
FROM nginx:alpine

# Clean out default Nginx assets
RUN rm -rf /usr/share/nginx/html/*

# Copy build assets from Stage 1
COPY --from=builder /app/dist /usr/share/nginx/html

# Copy custom Nginx proxy config
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
