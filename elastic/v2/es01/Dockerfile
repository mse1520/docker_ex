FROM elasticsearch:8.7.0
RUN mkdir config/es-certs
COPY instances.yml config/es-certs
RUN bin/elasticsearch-certutil ca --silent --pem -out config/es-certs/ca.zip
RUN unzip config/es-certs/ca.zip -d config/es-certs
RUN bin/elasticsearch-certutil cert --silent --pem -out config/es-certs/certs.zip --in config/es-certs/instances.yml --ca-cert config/es-certs/ca/ca.crt --ca-key config/es-certs/ca/ca.key
RUN unzip config/es-certs/certs.zip -d config/es-certs
