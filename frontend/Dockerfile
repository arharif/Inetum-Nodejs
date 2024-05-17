# Use the official Node.js 14 image as a base
FROM node:14 as build

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the frontend
RUN npm run build

# Use a lightweight Node.js image for the production environment
FROM node:14-alpine

# Set the working directory in the container
WORKDIR /app

# Copy the build output from the previous stage
COPY --from=build /app/dist ./dist

# Expose the port the app runs on (if applicable)
# EXPOSE 8080

# Command to run the application
CMD ["npx", "http-server", "-p", "8080", "./dist"]
