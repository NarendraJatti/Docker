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
 