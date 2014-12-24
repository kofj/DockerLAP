FROM docker.cn/docker/centos:centos6

MAINTAINER Frank Kung,"kfanjian@gmail.com"

# 环境变量
ENV LD_LIBRARY_PATH /usr/local/lib

# 安装程序，跳过 php-snmp
RUN yum update minimal \
	&& rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6 \
	&& yum install -y httpd httpd-devel wget tar libjpeg* cyrus-sasl-devel pcre-devel zlib-devel gcc gcc-c++ make php php-devel php-gd php-mysqlnd php-pdo php-pear php-pecl-memcache php-xml php-bcmath php-intl php-ldap php-mbstring php-odbc php-pgsql php-mysql php-soap php-xmlrpc --skip-broken \
	&& yum clean all \
	
	#创建临时文件夹
	&& mkdir -p /tmp/src \
	&& cd /tmp/src \
	
	# 安装libmcrypt
	&& wget http://192.168.10.101/Project/files/OpenSourceLib/LAMP/libmcrypt-2.5.8.tar.gz \
	&& tar -xvzf libmcrypt-2.5.8.tar.gz \
	&& cd libmcrypt-2.5.8 \
	&& ./configure \
	&& make && make install \
	&& cd /tmp/src && rm -fr libmcrypt-2.5.8 \
	
	#安装mhash
	&& wget http://192.168.10.101/Project/files/OpenSourceLib/LAMP/mhash-0.9.9.9.tar.gz \
	&& tar -xvzf mhash-0.9.9.9.tar.gz \
	&& cd mhash-0.9.9.9 \
	&& ./configure \
	&& make && make install \
	&& cd /tmp/src && rm -fr mhash-0.9.9.9 \
	
	#安装mcrypt
	&& wget http://192.168.10.101/Project/files/OpenSourceLib/LAMP/mcrypt-2.6.8.tar.gz \
	&& tar -xvzf mcrypt-2.6.8.tar.gz \
	&& cd mcrypt-2.6.8 \
	&& ./configure \
	&& make && make install \
	&& cd /tmp/src && rm -fr mcrypt-2.6.8 \
	
	#安装php-mcrypt
	#&& cd /tmp/src && wget http://museum.php.net/php5/php-5.3.3.tar.gz \
	&& cd /tmp/src && wget http://192.168.10.101/Project/files/php-5.3.3.tar.gz \
	&& tar -xvzf php-5.3.3.tar.gz \
	&& cd /tmp/src/php-5.3.3/ext/mcrypt \
	&& phpize \
	#&& php-config \
	&& ./configure \
	&& make && make install \
	&& echo "extension=mcrypt.so" > /etc/php.d/mcrypt.ini \
	&& cd /tmp/src && rm -fr php-5.3.3 \

	#安装Oracle 10g扩展
	&& rpm -i http://192.168.10.101/Project/files/OracleFiles/database/10.2/oracle-instantclient-basic-10.2.0.5-1.x86_64.rpm \
	&& rpm -i http://192.168.10.101/Project/files/OracleFiles/database/10.2/oracle-instantclient-devel-10.2.0.5-1.x86_64.rpm \
	&& pecl install oci8 \
	&& echo "extension=oci8.so">/etc/php.d/oci8.ini \

	# 其它扩展
	&& pecl install redis \
	&& echo "extension=redis.so" > /etc/php.d/redis.ini \
	&& pecl install apc \
	&& echo "extension=apc.so" > /etc/php.d/apc.ini \
	&& pecl install mongo \
	&& echo "extension=mongo.so" > /etc/php.d/mongo.ini \

	# libmemecached 1.0.2 from source
	&& wget http://launchpad.net/libmemcached/1.0/1.0.2/+download/libmemcached-1.0.2.tar.gz \
	&& tar -zxvf libmemcached-1.0.2.tar.gz \
	&& cd libmemcached-1.0.2 \
	&& ./configure --prefix=/usr/local/libmemcached --enable-sasl \
	&& make  && make install \
	&& cd .. && rm -fr libmemcached* \

	# memcached 
	&& pecl install memcached \
	&& wget http://pecl.php.net/get/memcached-2.0.0.tgz \
	&& tar -zxvf memcached* \
	&& cd memcached* \
	&& ls -al \
	&& phpize \
	&& ./configure  --with-libmemcached-dir=/usr/local/libmemcached --enable-memcached-sasl \
	&& make && make install \
	&& cd  .. && rm -fr memcached* \
	&& echo "extension=memcached.so" > /etc/php.d/memcached.ini \


	# 卸载编译工具
	#&& yum remove -y gcc gcc-c++ kernel-headers libjpeg-turbo-devel apr-devel apr-util-devel cyrus-sasl-devel db4-devel expat-devel glibc-devel glibc-headers libstdc++-devel openldap-devel \

	# 清理无用文件
	&& rm -fr /tmp/pear \
	&& rm -fr /tmp/src \
	&& rm -fr /tmp \
	#&& rm -fr /var/lib/rpm/* \
	&& rm -fr /var/cache/* \
	&& rm -fr /usr/lib/locale/locale-archive \
	&& rm -fr /etc/httpd/conf.d/welcome.conf \
	&& rm -fr

	# 修改工作目录所属
	&& chown apache:apache -R /var/www/html

# 拷贝配置文件
COPY httpd.conf /etc/httpd/conf/
COPY index.html /var/www/html/
COPY index.html /var/www/error/noindex.html
COPY html /var/www/error

# 设置工作目录
WORKDIR /var/www/html

# 设置端口
EXPOSE 80

# 默认执行
CMD ["/usr/sbin/httpd", "-DFOREGROUND"]
