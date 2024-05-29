# Base image from the official Node.js repository
FROM node:14

# Set the working directory within the container
WORKDIR /usr/src/app

# Copy the package.json and package-lock.json files
COPY package*.json ./

# Install the dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Expose port 80 for the application
EXPOSE 80

# Command to run the application
CMD ["node", "server.js"]
