# Use an official Node runtime as a parent image
FROM node:20-alpine AS builder

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json first to leverage Docker cache
COPY package*.json ./

# Install dependencies
RUN npm install --legacy-peer-deps

# Copy the rest of the application code
COPY . .

# Build the application
RUN npm run build

# Stage 2: Serve the app (or just keep it minimal for artifact extraction)
# Since we are extracting files for S3, we just need the builder stage really,
# but usually a valid Dockerfile produces a runnable image.
# We'll use a lightweight server just in case they want to run it elsewhere.
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
