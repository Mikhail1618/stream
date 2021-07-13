FROM alpine:3.14.0 AS builder

ENV TZ=Europe/Minsk
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apk update && apk upgrade \
    && apk add gcc make libc-dev openssl-dev zlib-dev pcre-dev

COPY nginx-1.20.1 /root/nginx-1.20.1
COPY nginx-rtmp-module-master /root/nginx-rtmp-module-master

WORKDIR /root/nginx-1.20.1
RUN ./configure --prefix=/var/lib/nginx --sbin-path=/usr/sbin/nginx \
--modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf \
--pid-path=/run/nginx/nginx.pid --lock-path=/run/nginx/nginx.lock \
--http-client-body-temp-path=/var/lib/nginx/tmp/client_body \
--with-perl_modules_path=/usr/lib/perl5/vendor_perl \
--user=nginx --group=nginx --with-threads \
--with-http_v2_module --with-http_gunzip_module \
--with-http_gzip_static_module --with-http_degradation_module \
--add-module=../nginx-rtmp-module-master --without-http_rewrite_module

WORKDIR /root/nginx-1.20.1 
RUN make && make install


FROM alpine:3.14.0

RUN apk update && apk upgrade && apk add nginx

COPY --from=builder /usr/sbin/nginx /usr/sbin/nginx

WORKDIR /run/nginx
RUN echo "999" > /run/nginx/nginx.pid

WORKDIR /srv/stream
COPY  ./hls.js ./index.html ./
COPY ./nginx.conf /etc/nginx/nginx.conf

CMD ["nginx", "-g", "daemon off;"]
