FROM mysql 

#Copia el script init.sql al directorio de inicialización de MySQL
COPY init.sql /docker-entrypoint-initdb.d/

# Define las variables de entorno (opcional)
ENV MYSQL_ROOT_PASSWORD = root

