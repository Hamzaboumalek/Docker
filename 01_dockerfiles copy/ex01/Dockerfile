FROM debian
RUN apt-get -y update && apt-get -y upgrade && \
		apt-get -y install wget bzip2 && \
		wget https://files.teamspeak-services.com/releases/server/3.11.0/teamspeak3-server_linux_amd64-3.11.0.tar.bz2 && \
		tar xvf teamspeak3-server_linux_amd64-3.11.0.tar.bz2
ENV TS3SERVER_LICENSE=accept
EXPOSE 9987/udp 10011 30033
WORKDIR teamspeak3-server_linux_amd64
CMD ["./ts3server"]
