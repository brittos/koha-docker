FROM debian:12


RUN apt update && apt install -y \
  wget \
  gnupg2 \
  mariadb-client \
  perlbrew

RUN wget -qO - https://debian.koha-community.org/koha/gpg.asc | apt-key add -

RUN echo 'deb https://debian.koha-community.org/koha stable main' | tee /etc/apt/sources.list.d/koha.list

RUN apt update && apt install -y \
  koha-common

RUN perlbrew install-cpanm

RUN /root/perl5/perlbrew/bin/cpanm install JSON::Validator

RUN /root/perl5/perlbrew/bin/cpanm install Mojolicious::Plugin::OpenAPI

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

RUN a2enmod rewrite cgi mpm_itk

# Koha specific stuff

COPY koha-sites.conf /etc/koha/koha-sites.conf

COPY koha-common.cnf /etc/mysql/koha-common.cnf

RUN koha-translate --install pt-BR

COPY entrypoint.sh /

RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
