FROM centos:6

# system update
RUN yum -y update && yum clean all

# set locale
RUN yum reinstall -y glibc-common && yum clean all
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8

# epel repo
RUN yum install -y http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm && \
    yum clean all

# remi repo
RUN yum install -y http://rpms.famillecollet.com/enterprise/remi-release-6.rpm && \
    yum clean all

# httpd
RUN yum install -y httpd httpd-tools && \
    yum clean all

# gd-last (for php-gd)
RUN yum install --enablerepo=remi -y gd-last && \
    yum clean all

# php install
RUN yum install --enablerepo=remi,remi-php56 -y \
    php \
    php-devel \
    php-mbstring \
    php-pdo \
    php-mysqlnd \
    php-gd \
    php-xml \
    php-pear \
    php-mcrypt && \
    yum clean all

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version
RUN composer g require psy/psysh:@stable
ENV PATH ~/.composer/vendor/bin:$PATH

RUN pear install MDB2
RUN pear install MDB2_Driver_mysql

# conf setting
COPY ./httpd.conf /etc/httpd/conf/httpd.conf
COPY ./php.ini /etc/php.ini

CMD ["/usr/sbin/httpd","-D","FOREGROUND"]

