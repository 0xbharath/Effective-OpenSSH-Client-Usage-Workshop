FROM bitnami/minideb:latest

# Install OpenSSH server
RUN apt-get update && \
    apt-get install -y openssh-server nano net-tools && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir /var/run/sshd

# Copy public key to container
COPY ./id.pub /root/.ssh/authorized_keys

# Set permissions on the authorized_keys file
RUN chmod 600 /root/.ssh/authorized_keys

RUN touch /etc/hosts.allow && touch /etc/hosts.deny
RUN echo "sshd: ALL" >> /etc/hosts.deny
RUN echo "sshd: 10.10.0.4/32" >> /etc/hosts.allow

# Set SSH client IP restriction
#RUN echo "Match Address 172.17.0.2" >> /etc/ssh/sshd_config && \
#    echo "ForceCommand echo 'This account can only be accessed from 172.17.0.2'" >> /etc/ssh/sshd_config

# Disable password authentication and root login
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/g' /etc/ssh/sshd_config
#    sed -i 's/PermitRootLogin yes/PermitRootLogin no/g' /etc/ssh/sshd_config

# Expose port 22 for SSH
EXPOSE 22

# Start SSH server
CMD ["/usr/sbin/sshd", "-D"]


