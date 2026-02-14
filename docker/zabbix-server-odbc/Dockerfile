# Zabbix server with unixODBC and PostgreSQL ODBC driver for Database monitor items.
# Base image is Alpine-based; driver path may vary on aarch64 - if odbcinst -q -d
# fails, run: find /usr -name "*psqlodbc*" inside the container and update odbcinst.ini.
FROM zabbix/zabbix-server-pgsql:latest

USER root

RUN apk add --no-cache unixodbc psqlodbc

COPY odbcinst.ini /etc/odbcinst.ini
COPY odbc.ini /etc/odbc.ini

USER 1997
