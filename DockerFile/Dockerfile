#  FROM python:3.14.0a5-bookworm
# FROM scratch

FROM alpine

LABEL CREATOR="naren"

RUN echo hello #shell format

# RUN ["/bin/sh,"-c","echo hello"] #exec format

RUN apk add curl 

WORKDIR /downloads 

# Use Alpine as the base image
FROM alpine

# Print hello during build process (shell format)
RUN echo hello

# Install curl in the container
RUN apk add curl

# Set the working directory to /download
WORKDIR /code

# Create a test file named testfile.txt with some content
RUN echo "This is a test file" > testfile.txt

RUN adduser -D naren

USER naren

ENV app_port = 5000 \
    app_host = '0.0.0.0'

COPY app.py /code/

# ADD htpps://mybucket.s3.amazonaws.com/token.txt .

EXPOSE 5000

# CMD ["npm","start"] or beloway

# ENTRYPOINT [ "npm" ]
# CMD [ "start" ]
 

#below is multi-stage build>>for react app
#docker histoy imagename>to chekc layers
# Stage 1: Build Stage
FROM node:18-alpine AS builder

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application source code
COPY . .

# Build the React app
RUN npm run build

# Stage 2: Production Stage
FROM nginx:alpine

# Remove default Nginx website
RUN rm -rf /usr/share/nginx/html/*

# Copy built React app from Stage 1
COPY --from=builder /app/build /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]

Explanation of Steps:
Stage 1: Build Stage

FROM node:18-alpine AS build: Uses a lightweight Node.js base image to build the React app.
WORKDIR /app: Sets /app as the working directory inside the container.
COPY package*.json ./: Copies the package.json and package-lock.json files to the container. These files contain the app's dependencies.
RUN npm install: Installs the app’s dependencies.
COPY . .: Copies the rest of the React app’s source code into the container.
RUN npm run build: Runs the build script, which generates the production-ready static files for the React app inside the build/ folder.
Stage 2: Production Stage

FROM nginx:alpine: Uses a minimal Nginx image to serve the built app.
RUN rm -rf /usr/share/nginx/html/*: Cleans the default Nginx HTML files.
COPY --from=build /app/build /usr/share/nginx/html: Copies the production build files generated in Stage 1 into the Nginx web server’s default directory (/usr/share/nginx/html).
EXPOSE 80: Exposes port 80 so the app can be accessed via HTTP.
CMD ["nginx", "-g", "daemon off;"]: Starts the Nginx server in the foreground to serve the app.
Why Multi-Stage Build is Beneficial:
Size Optimization: In the first stage, all the necessary tools (like Node.js, npm, etc.) are used to build the app, but you don’t need them for running the app in production. The second stage only copies the final build files and uses Nginx to serve them, making the final image significantly smaller and more lightweight.

No Extra Tools in Production: The Nginx image in the second stage only contains the minimal requirements to serve static files (the built React app). Since it doesn’t contain Node.js or the source code, the final production image is much more secure and efficient.

This multi-stage build ensures that the build dependencies and development files aren’t included in the production image, minimizing the container size and security risks.







