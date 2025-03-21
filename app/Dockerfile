# Use the official Node.js 20 Alpine image
FROM node:20-alpine

# Set the working directory
WORKDIR /usr/src/app

# Install a compatible npm version (skip Yarn)
RUN npm install -g npm@latest pm2

# Copy package.json and package-lock.json
COPY package.json package-lock.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application
COPY . .

# Expose port 3000
EXPOSE 3000

# Start the application
CMD ["npm", "start"]
