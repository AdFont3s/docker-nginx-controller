FROM nginx-plus
 
ARG cert=nginx-repo.crt
ARG key=nginx-repo.key

# COPY $cert /etc/ssl/nginx/
# COPY $key /etc/ssl/nginx/

# e.g '1234567890'
ARG API_KEY 
ENV ENV_API_KEY=$API_KEY

# e.g https://<fqdn>:8443/1.4
ARG CONTROLLER_URL
ENV ENV_CONTROLLER_URL=$CONTROLLER_URL

# Install everything we will need to install the Controller Agent so that the container can start quickly
RUN apt-get update && apt install -y curl python gnupg2 procps dh-python distro-info-data libmpdec2 libpython3-stdlib libpython3.5-minimal libpython3.5-stdlib lsb-release python3 python3-minimal python3.5 python3.5-minimal
 
EXPOSE 80 443 8080 8090
STOPSIGNAL SIGTERM
 
WORKDIR /controller
RUN printf "curl -skSL ${CONTROLLER_URL} | bash -s - -y\nnginx -g 'daemon off;'" > start
CMD ["sh", "/controller/start"]