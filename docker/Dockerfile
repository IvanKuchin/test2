############
#  Stage 1 #
############
FROM alpine as builder
RUN apk add hugo git

# Date renders incorrectly in the container, so we need to set the timezone
# FROM ubuntu as builder
# RUN apt-get update && apt-get install -y hugo git

# Set destination for COPY
WORKDIR /app
COPY . .
RUN git clone https://github.com/nicokaiser/hugo-theme-gallery/ themes/gallery

RUN hugo

############
#  Stage 2 #
############
FROM busybox:latest as app

# Create a non-root user to own the files and run our server
# RUN adduser -D static
# USER static
WORKDIR /home/static

# Copy the static website
# Use the .dockerignore file to control what ends up inside the image!
COPY --from=builder /app/public/ .

EXPOSE 80

# Run BusyBox httpd
CMD ["busybox", "httpd", "-f", "-v", "-p", "80"]