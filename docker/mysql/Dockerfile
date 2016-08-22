FROM mysql:5.7
RUN apt-get update \
  && apt-get install -y locales --no-install-recommends \
  && rm -fr /var/lib/apt/lists/* \
  && echo "ja_JP.UTF-8 UTF-8" > /etc/locale.gen \
  && dpkg-reconfigure -f noninteractive locales
ENV MYSQL_USER=vagrant \
  MYSQL_PASSWORD=vagrant \
  LC_ALL=ja_JP.UTF-8
COPY etc/mysql/conf.d /etc/mysql/conf.d/

