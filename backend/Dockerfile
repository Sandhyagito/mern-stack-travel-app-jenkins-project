# Use an official Node.js image as the base
FROM node:14

# Set the working directory inside the container
WORKDIR /app

# Copy only the package.json and package-lock.json files to install dependencies
COPY package.json ./

# Install the dependencies for the Node.js app
RUN npm install

# Copy the rest of the application files into the container
COPY . .

# Expose port 5000 (the port your Node.js backend runs on)
EXPOSE 5000

# Start the Node.js API server
CMD ["node", "server.js"]
