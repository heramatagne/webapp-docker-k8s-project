# Specify the Base Image
FROM centos:7

# Add metadata indicating the maintainer
LABEL maintainer="my docker container"

# Update packages and install Apache
RUN yum -y update
RUN yum -y install httpd && yum clean all    

# Copy the entire folder into the container
# COPY . /var/www/html/
COPY ./webapp/ /var/www/html/

# Expose port 80 to the outside world
EXPOSE 80

# Set the entry point
ENTRYPOINT ["/usr/sbin/httpd"]

# Start Apache in the foreground when container starts
CMD ["-D", "FOREGROUND"]
