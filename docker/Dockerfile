FROM ubuntu:xenial

# installs all required packages
RUN apt-get update \
	&& apt-get install -y software-properties-common python-software-properties \
	&& echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections \
	&& add-apt-repository ppa:webupd8team/java -y \
	&& apt-get update \
	&& apt-get install -y oracle-java8-installer oracle-java8-set-default libarchive13 libcurl3 ocaml ocaml-native-compilers camlp4 git build-essential zip python-pip\
	&& wget http://131.123.42.38/lmcrs/beta/srcML-Ubuntu14.04-64.deb \
	&& dpkg -i srcML-Ubuntu14.04-64.deb \
	&& pip install jsontree asttokens

# install cgum
WORKDIR /opt
RUN git clone https://github.com/GumTreeDiff/cgum.git --depth 1
WORKDIR /opt/cgum
RUN make \
	&& ln -s /opt/cgum/cgum /usr/bin/cgum

# install pythonparser
WORKDIR /opt
RUN git clone https://github.com/GumTreeDiff/pythonparser.git --depth 1
WORKDIR /opt/pythonparser
RUN ln -s /opt/pythonparser/pythonparser /usr/bin/pythonparser

# install gumtree
WORKDIR /opt
RUN git clone -b develop https://github.com/GumTreeDiff/gumtree.git --depth 1
WORKDIR /opt/gumtree
RUN ./gradlew build -x check \
	&& unzip dist/build/distributions/gumtree.zip \
	&& ln -s /opt/gumtree/gumtree/bin/gumtree /usr/bin/gumtree

# define volume diff to make available files to diff
RUN mkdir /diff
WORKDIR /diff
VOLUME /diff

# expose port 4567 for webdiff
EXPOSE 4567

ENTRYPOINT ["gumtree"]
