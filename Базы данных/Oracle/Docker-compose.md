---
tags:
  - docker
  - database
  - oracle
---
```docker
version: '3.8'

services:

  oracle-db:

    image: gvenzl/oracle-xe

    container_name: oracle-xe

    ports:

      - "1521:1521"

    environment:

      - ORACLE_PASSWORD=admin

      - ORACLE_DATABASE=db

      - APP_USER=admin

      - APP_USER_PASSWORD=admin

    volumes:

      - oracle-data:/opt/oracle/oradata

    restart: unless-stopped

  

volumes:

  oracle-data:
```
