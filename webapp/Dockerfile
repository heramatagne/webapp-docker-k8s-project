# Specify the Base Image
FROM centos:7

# Add metadata indicating the maintainer
LABEL maintainer="my docker container"

# Update packages and install Apache
RUN yum -y update
RUN yum -y install httpd && yum clean all    

# Create a directory for the web application
WORKDIR /var/www/html/

# Copy necessary files into the container
COPY webapp/index.html ./index.html
COPY webapp/license.txt ./license.txt
COPY webapp/js/ ./js/
COPY webapp/img/ ./img/
COPY webapp/css/ ./css/
COPY webapp/fonts/ ./fonts/

# Expose port 80 to the outside world
EXPOSE 80

# Set the entry point
ENTRYPOINT ["/usr/sbin/httpd"]

# Start Apache in the foreground when container starts
CMD ["-D", "FOREGROUND"]
