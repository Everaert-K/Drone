# Base image for x64/x86
FROM node:boron				

# Create app directory
RUN mkdir -p /usr/src/backend

# Directory containing package.json file
COPY src/ /usr/src/backend/ 

WORKDIR /usr/src/backend

# Install dependencies
RUN npm install

# Expose HTTP port
EXPOSE 3000

# Start app through npm
CMD ["npm", "start"]