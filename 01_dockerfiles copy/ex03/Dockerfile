FROM ubuntu:latest
RUN apt-get update  && apt-get upgrade -y &&\
		DEBIAN_FRONTEND=noninteractive apt-get install apt-utils apt-transport-https curl openssh-server ca-certificates postfix -y && \
		DEBIAN_FRONTEND=noninteractive curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash && \
		DEBIAN_FRONTEND=noninteractive apt-get install -y gitlab-ce && \
		DEBIAN_FRONTEND=noninteractive apt-get install -y tzdata
RUN	mkdir -p /etc/gitlab/ssl && chmod 700 /etc/gitlab/ssl
RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/gitlab/ssl/gitlab.key -out /etc/gitlab/ssl/gitlab.crt -subj "/C=MA/ST=BK/L=KHOURIBGA/O=1337/CN=10.12.7.6"
WORKDIR /etc/gitlab
RUN sed -i 's/http:\/\/gitlab.example.com/https:\/\/10.12.7.6/' gitlab.rb && \
			echo "nginx['enable'] = true" >> gitlab.rb && \
			echo "nginx['redirect_http_to_https'] = true" >> gitlab.rb && \
			echo "nginx['redirect_http_to_https_port'] = 80" >> gitlab.rb && \
			echo "nginx['ssl_certificate'] = '/etc/gitlab/ssl/gitlab.crt'" >> gitlab.rb && \
			echo "nginx['ssl_certificate_key'] = '/etc/gitlab/ssl/gitlab.key'" >> gitlab.rb && \
			echo "gitlab_rails['gitlab_shell_ssh_port'] = 2222" >> gitlab.rb && \
			echo "Port 2222" >> /etc/ssh/sshd_config
EXPOSE 80 443 2222
ENTRYPOINT /opt/gitlab/embedded/bin/runsvdir-start& gitlab-ctl reconfigure && service ssh restart && gitlab-ctl tail
