FROM debian:10

RUN apt-get update && \
	apt-get install -y git clang cmake make gcc g++ libmariadbclient-dev libssl-dev libbz2-dev libreadline-dev libncurses-dev libboost-all-dev mariadb-server p7zip default-libmysqlclient-dev wget && \
	update-alternatives --install /usr/bin/cc cc /usr/bin/clang 100 && \
	update-alternatives --install /usr/bin/c++ c++ /usr/bin/clang 100 && \
	rm -rf /var/lib/apt/lists/*

RUN cd ~/ && \
	git clone -b 3.3.5 --depth 1 git://github.com/TrinityCore/TrinityCore.git && \
	cd ~/TrinityCore && \
	mkdir build && \
	cd build && \
	cmake ../ -DCMAKE_INSTALL_PREFIX=/server && \
	make -j $(nproc) install && \
	cd .. && \
	mv sql .. && \
	rm -rf * && \
	mv ../sql .

RUN mkdir ~/TDB && \
	cd ~/TDB && \
	wget https://github.com/TrinityCore/TrinityCore/releases/download/TDB335.19111/TDB_full_world_335.19111_2019_11_16.7z && \
	7zr x TDB_full_world_335.19111_2019_11_16.7z && \
	mv *.sql /server/bin && \
	cd / && \
	rm -rf ~/TDB
RUN mkdir ~/unrar && \
	cd ~/unrar && \
	wget http://www.rarlab.com/rar/unrarsrc-5.8.3.tar.gz && \
	tar zxvf unrarsrc-5.8.3.tar.gz && \
	cd unrar && \
	make -f makefile && \
	install -v -m755 unrar /usr/bin && \
	cd / && \
	rm -rf ~/unrar
RUN mkdir ~/ADB && \
	cd ~/ADB && \
	wget https://github.com/AshamaneProject/AshamaneCore/releases/download/ADB_735.10/ADB_735.10.rar && \
	unrar e ADB_735.10.rar && \
	mv *.sql /server/bin && \
	cd / && \
	rm -rf ~/ADB

RUN apt-get remove -y git clang cmake make gcc g++ libmariadbclient-dev libssl-dev libbz2-dev libreadline-dev libncurses-dev libboost-all-dev mariadb-server p7zip default-libmysqlclient-dev wget

VOLUME /data
VOLUME /config

VOLUME /data
VOLUME /config
VOLUME /logs

ADD entrypoint.sh /

ENTRYPOINT ["/entrypoint.sh"]
