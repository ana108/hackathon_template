FROM node:24.22.1-bookworm-slim AS build
# Install node-gyp requirements
RUN apt-get update && apt-get install -y \
    python3 \
    make \
    g++ \
    && rm -rf /var/lib/apt/lists/*
WORKDIR /app

# Copy package.json and package-lock.json (or yarn.lock) to leverage Docker cache
COPY package*.json ./
RUN npm ci

# Stage 2: Run the application with only production dependencies
FROM node:24.22.1-bookworm-slim AS run
WORKDIR /app

# Copy the rest of the application code
COPY . .

# Copy production node_modules from the build stage
COPY --from=build /app/node_modules ./node_modules

# Expose the port your app runs on
EXPOSE 3000

# Command to run the application (e.g., start the compiled JS file)
# Adjust the file path to where your application starts (e.g., dist/index.js or src/index.js)
CMD ["node", "server.js"]