FROM node:lts-slim

RUN echo "alias ll='ls -alhG'" >> /root/.bashrc

WORKDIR /usr/src/app

RUN apt-get update && \
    apt-get install -y git

RUN npm install express mongoose nodemon --save

# COPY entrypoint.sh /usr/local/bin/entrypoint.sh

# ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]