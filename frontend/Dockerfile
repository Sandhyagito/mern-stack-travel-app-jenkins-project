# Use an official Node.js image as the base
FROM node:14

# Set the working directory inside the container
WORKDIR /app

# Copy only the package.json and package-lock.json files to install dependencies
COPY package.json ./

# Install the dependencies for the React app
RUN npm install

# Copy the rest of the application files into the container
COPY . .

# Build the React app (this generates the static files)
RUN npm run build

# Expose port 3000 (React development server runs on this port by default)
EXPOSE 3000

# Start the React app in development mode
CMD ["npm", "start"]
