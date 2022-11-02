#ARG NGINX_VERSION=1.22

FROM nginx:1.22

RUN apt-get update && apt-get install -y \
	nginx-module-geoip \
	wget \
	libssl-dev \
	build-essential \
        libpcre++-dev \
        zlib1g-dev \
	git \
        libgeoip-dev \
        libmaxminddb-dev \
	&& rm -rf /var/lib/apt/lists/*; \
	mkdir /usr/share/geoip; cd /usr/share/geoip/ 
#	wget https://mailfud.org/geoip-legacy/GeoIP.dat.gz; \
#	gunzip GeoIP.dat.gz;

# FIXME: use nginx -V to use current compile options
#        NGINX_OPTIONS=$(2>&1 nginx -V | grep 'configure arguments' | awk -F: '{print $2}') \

ENV MAXMIND_VERSION=1.6.0
RUN set -x \
  && wget https://github.com/maxmind/libmaxminddb/releases/download/${MAXMIND_VERSION}/libmaxminddb-${MAXMIND_VERSION}.tar.gz \
  && tar xf libmaxminddb-${MAXMIND_VERSION}.tar.gz \
  && cd libmaxminddb-${MAXMIND_VERSION} \
  && ./configure \
  && make \
  && make check \
  && make install \
  && ldconfig

RUN cd /opt \
    && git clone  https://github.com/leev/ngx_http_geoip2_module.git \
    && wget -O - http://nginx.org/download/nginx-$NGINX_VERSION.tar.gz | tar zxfv - ; \
    ls -la /opt ; \
    mv /opt/nginx-$NGINX_VERSION /opt/nginx ; \
    cd /opt/nginx; \
    ./configure --with-compat --add-dynamic-module=/opt/ngx_http_geoip2_module \
#    ./configure \
#    --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-g -O2 -ffile-prefix-map=/data/builder/debuild/nginx-1.22.0/debian/debuild-base/nginx-1.22.0=. -fstack-protector-strong -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -fPIC' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -Wl,--as-needed -pie' \
#    --add-dynamic-module=/opt/ngx_http_geoip2_module \
#--with-compat --with-http_geoip_module; \
    && make modules \
    && cp objs/ngx_http_geoip2_module.so /etc/nginx/modules/ngx_http_geoip2_module.so
