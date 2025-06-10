# syntax=docker/dockerfile:1
FROM debian as build
WORKDIR /src

RUN apt update -y \
&& apt install wget gcc make libpcre3-dev zlib1g-dev -y \ 
&& wget https://nginx.org/download/nginx-1.27.4.tar.gz \
&& tar -zxf nginx-1.27.4.tar.gz \
&& cd nginx-1.27.4 \
&& ./configure --with-ld-opt="-static" --with-http_sub_module \ 
&& make install \
&& strip /usr/local/nginx/sbin/nginx

FROM scratch

COPY --from=build /etc/passwd /etc/group /etc/
COPY --from=build /usr/local/nginx /usr/local/nginx
COPY index.html /usr/share/nginx/html/
COPY nginx.conf /usr/local/nginx/conf/

EXPOSE 80

ENTRYPOINT ["/usr/local/nginx/sbin/nginx"]
CMD ["-g", "daemon off;"]
