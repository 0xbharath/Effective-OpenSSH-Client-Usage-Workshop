FROM bitnami/minideb:latest

# Install OpenSSH server
RUN apt-get update && \
    apt-get install -y openssh-server nano net-tools nginx curl python3 && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir /var/run/sshd

# Copy public key to container
COPY ./id.pub /root/.ssh/authorized_keys

# Set permissions on the authorized_keys file
RUN chmod 600 /root/.ssh/authorized_keys

#RUN touch /etc/hosts.allow && touch /etc/hosts.deny
#RUN echo "sshd: ALL" >> /etc/hosts.deny
#RUN echo "sshd: 172.17.0.2/32" >> /etc/hosts.allow

# Set SSH client IP restriction
#RUN echo "Match Address 172.17.0.2" >> /etc/ssh/sshd_config && \
#    echo "ForceCommand echo 'This account can only be accessed from 172.17.0.2'" >> /etc/ssh/sshd_config

# Disable password authentication and root login
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/g' /etc/ssh/sshd_config
#    sed -i 's/PermitRootLogin yes/PermitRootLogin no/g' /etc/ssh/sshd_config

RUN echo "allow 10.10.0.7;" >> /etc/nginx/conf.d/whitelist.conf

RUN echo "server { \
    listen 10.10.0.8:80; \
    location / { \
        return 200 'Your IP Address is: \$remote_addr \n'; \
    } \
}" > /etc/nginx/sites-available/default

RUN touch /etc/hosts.allow && touch /etc/hosts.deny
RUN echo "sshd: ALL" >> /etc/hosts.deny
RUN echo "sshd: 10.10.0.7/32" >> /etc/hosts.allow

# Expose port 22 for SSH
EXPOSE 22 80 8080

COPY ./setup.sh /home/
RUN chmod +x /home/setup.sh
# Start SSH server
#CMD ["/usr/sbin/sshd", "-D"]

CMD ["/bin/bash", "-c", "/usr/sbin/sshd -D & nginx -g 'daemon off;' & python3 -m http.server 8080 --bind 127.0.0.1"]



