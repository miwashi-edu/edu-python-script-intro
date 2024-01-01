# edu-python-script-intro

## Dockerfile

```bash
mkdir kali_python && cd kali_python
cat > Dockerfile << 'EOF'
FROM kalilinux/kali-rolling

# Update and upgrade the system
RUN apt-get update && apt-get upgrade -y && apt-get install -y git

# Install necessary packages for development
RUN apt-get install -y vim sudo python3 python3.11-venv pip
RUN apt-get clean

# Install a Python linter (e.g., pylint)
RUN pip install pylint

# Set the username as a build argument (default to 'devuser')
ARG USERNAME=devuser

# Create a non-root user with sudo rights
RUN useradd -ms /bin/bash $USERNAME && \
    usermod -aG sudo $USERNAME

# Set a password for the user (you can change 'password' to your desired password)
RUN echo "$USERNAME:password" | chpasswd

# Create the home directory for the user and set it as the user's home
RUN mkdir -p /home/$USERNAME/ws && \
    chown $USERNAME:$USERNAME /home/$USERNAME/ws && \
    usermod -d /home/$USERNAME/ws $USERNAME

# Switch to the user
USER $USERNAME

# Set the working directory to the user's home
WORKDIR /home/$USERNAME/ws
EOF

docker build -t pydev:latest .
docker run -d -p 5000:80 -h pydev --name pydev pydev:latest tail -f /dev/null
```


## Hello World

```bash

```

