# Specify the Base Image
FROM centos:7

# Add metadata indicating the maintainer
LABEL maintainer="my docker container"

# Update packages and install Apache
RUN yum -y update && \
    yum -y install httpd && \
    yum clean all

# Create a directory for the web application
WORKDIR /var/www/html/

# Assuming webapp directory exists in the same directory as Dockerfile
# Copy necessary files into the container
COPY ./webapp/ .

# Expose port 80 to the outside world
EXPOSE 80

# Set the entry point
ENTRYPOINT ["/usr/sbin/httpd"]

# Start Apache in the foreground when container starts
CMD ["-D", "FOREGROUND"]
