# ==========================================================
# Stage 1: Build
# Use Node.js 20 (Alpine) to install dependencies and build the app
# ==========================================================
FROM node:20-alpine AS build

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json first (for better layer caching)
COPY package*.json ./

# Install dependencies using the lockfile (faster, reproducible builds)
RUN npm ci

# Copy the rest of the application code to the container
COPY . .

# Build the production-ready static files
RUN npm run build


# ==========================================================
# Stage 2: Production
# Serve the built static files using a lightweight Nginx image
# ==========================================================
FROM nginx:1.27-alpine

# Copy built static files from the build stage into Nginx's serving directory
COPY --from=build /app/build /usr/share/nginx/html

# Expose the port Nginx listens on
EXPOSE 80

# Run Nginx in the foreground (required for Docker containers)
CMD ["nginx", "-g", "daemon off;"]
