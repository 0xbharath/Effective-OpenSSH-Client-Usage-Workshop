FROM bitnami/minideb:latest

# Install OpenSSH server
RUN apt-get update && \
    apt-get install -y openssh-server net-tools nano && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir /var/run/sshd

# Copy public key to container
COPY ./id.pub /tmp/

# Configure SSH server for user "ubuntu"
RUN useradd -ms /bin/bash ubuntu && \
    mkdir -p /home/ubuntu/.ssh && \
    chmod 700 /home/ubuntu/.ssh && \
    cat /tmp/id.pub >> /home/ubuntu/.ssh/authorized_keys && \
    chmod 600 /home/ubuntu/.ssh/authorized_keys && \
    chown -R ubuntu:ubuntu /home/ubuntu/.ssh

COPY ./sample-directory/ /home/ubuntu/sample-directory/

#RUN chmod 700 /home/ubuntu/.ssh/id

# Set SSH client IP restriction
#RUN echo "Match Address 172.17.0.2" >> /etc/ssh/sshd_config && \
#    echo "ForceCommand echo 'This account can only be accessed from 172.17.0.2'" >> /etc/ssh/sshd_config

# Disable password authentication and root login
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/g' /etc/ssh/sshd_config && \
    sed -i 's/PermitRootLogin yes/PermitRootLogin no/g' /etc/ssh/sshd_config

# Expose port 22 for SSH
EXPOSE 22
# Start SSH server
CMD ["/usr/sbin/sshd", "-D"]