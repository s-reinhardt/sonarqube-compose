FROM openjdk:8-jre-slim

MAINTAINER Timo Pagel <dependencycheckmaintainer@timo-pagel.de>

ENV version_url=https://jeremylong.github.io/DependencyCheck/current.txt
ENV download_url=https://dl.bintray.com/jeremy-long/owasp

RUN apt-get update                                                          && \
    apt-get install -y --no-install-recommends wget ruby mono-runtime       && \
    gem install bundle-audit                                                && \
    gem cleanup

RUN wget -O /tmp/current.txt ${version_url}                                 && \
    version=$(cat /tmp/current.txt)                                         && \
    file="dependency-check-${version}-release.zip"                          && \
    wget "$download_url/$file"                                              && \
    unzip ${file}                                                           && \
    rm ${file}                                                              && \
    mv dependency-check /usr/share/                                         && \
    apt-get remove --purge -y wget                                          && \
    apt-get autoremove -y                                                   && \
    rm -rf /var/lib/apt/lists/* /tmp/*
 
RUN mkdir /src
RUN mkdir /report

VOLUME ["/src" "/usr/share/dependency-check/data" "/report"]

WORKDIR /src

CMD ["--project", "OWASP Dependecy Check Project", "--scan", "/src", "--out", "/report"]
ENTRYPOINT ["/usr/share/dependency-check/bin/dependency-check.sh"]
