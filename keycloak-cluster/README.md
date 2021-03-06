### Keycloak Cluster Image

#### docker-compose.yml
```
$docker-compose up -d
```

```
version: "3.8"
services:
  ng:
    image: nginx
    ports: 
      - 80:80
      - 8080:80
    networks: 
      kc_net:
        ipv4_address: 10.0.0.2
    depends_on: 
      - kc1
      - kc2
      - kc3
    volumes: 
      - ./conf/nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./conf/nginx/default.conf:/etc/nginx/conf.d/default.conf
  db:
    image: mariadb
    ports: 
      - 3306:3306
      - 33060:33060
    networks: 
      kc_net:
        ipv4_address: 10.0.0.3
    # volumes:
    #   - mysql_data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: keycloak
      MYSQL_USER: keycloak
      MYSQL_PASSWORD: password
  kc1:
    image: wangsying/keycloak
    depends_on: 
      - db
    networks: 
      kc_net:
        ipv4_address: 10.0.0.4
    environment: 
      KEYCLOAK_USER: admin
      KEYCLOAK_PASSWORD: admin 
      KEYCLOAK_FRONTEND_URL: http://localhost/auth
      KEYCLOAK_HOSTNAME: localhost
      DB_VENDOR: mariadb
      DB_ADDR: db
      DB_DATABASE: keycloak
      DB_USER: keycloak
      DB_PASSWORD: password
      PROXY_ADDRESS_FORWARDING: 'true'
      JGROUPS_DISCOVERY_EXTERNAL_IP: 10.0.0.4
      JGROUPS_DISCOVERY_PROTOCOL: JDBC_PING
      # JGROUPS_DISCOVERY_PROTOCOL: TCPPING
      # JGROUPS_DISCOVERY_PROPERTIES: initial_hosts="10.0.0.4[7600],10.0.0.5[7600]"
  kc2:
    image: wangsying/keycloak
    depends_on: 
      - db
    networks: 
      kc_net:
        ipv4_address: 10.0.0.5
    environment: 
      KEYCLOAK_USER: admin
      KEYCLOAK_PASSWORD: admin
      KEYCLOAK_FRONTEND_URL: http://localhost/auth
      KEYCLOAK_HOSTNAME: localhost
      DB_VENDOR: mariadb
      DB_ADDR: db
      DB_DATABASE: keycloak
      DB_USER: keycloak
      DB_PASSWORD: password
      PROXY_ADDRESS_FORWARDING: 'true'
      JGROUPS_DISCOVERY_EXTERNAL_IP: 10.0.0.5
      JGROUPS_DISCOVERY_PROTOCOL: JDBC_PING
      # JGROUPS_DISCOVERY_PROTOCOL: TCPPING
      # JGROUPS_DISCOVERY_PROPERTIES: initial_hosts="10.0.0.4[7600],10.0.0.5[7600]"
  kc3:
    image: wangsying/keycloak
    depends_on: 
      - db
    networks: 
      kc_net:
        ipv4_address: 10.0.0.6
    environment: 
      KEYCLOAK_USER: admin
      KEYCLOAK_PASSWORD: admin
      KEYCLOAK_FRONTEND_URL: http://localhost/auth
      KEYCLOAK_HOSTNAME: localhost
      DB_VENDOR: mariadb
      DB_ADDR: db
      DB_DATABASE: keycloak
      DB_USER: keycloak
      DB_PASSWORD: password
      PROXY_ADDRESS_FORWARDING: 'true'
      JGROUPS_DISCOVERY_EXTERNAL_IP: 10.0.0.6
      JGROUPS_DISCOVERY_PROTOCOL: JDBC_PING
      # JGROUPS_DISCOVERY_PROTOCOL: TCPPING
      # JGROUPS_DISCOVERY_PROPERTIES: initial_hosts="10.0.0.4[7600],10.0.0.5[7600]"
    
networks:
  kc_net:
    ipam:
      driver: default
      config:
        - subnet: "10.0.0.1/24"
```
