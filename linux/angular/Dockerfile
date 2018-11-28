FROM centos:7

RUN yum -y update
RUN yum -y install curl
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
RUN rpm -Uvh  http://rpms.remirepo.net/enterprise/remi-release-7.rpm

RUN curl --silent --location https://rpm.nodesource.com/setup_9.x | bash -

RUN ACCEPT_EULA=Y yum -y install httpd nodejs

RUN npm install -g @angular/cli

RUN yum -y install git

EXPOSE 80

CMD ["httpd", "-D", "FOREGROUND"]