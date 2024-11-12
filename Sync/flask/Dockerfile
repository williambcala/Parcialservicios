# Usa una imagen base oficial de Python
FROM python:3.9

# Establece el directorio de trabajo dentro del contenedor
WORKDIR /webapp

# Copia el archivo de requisitos y la aplicación Flask en el contenedor
COPY requirements.txt requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Copia el resto de los archivos de la aplicación
COPY ./webapp/ .

# Instala Nginx
RUN apt-get update && \
    apt-get install -y nginx  supervisor && \
    rm -rf /var/lib/apt/lists/*
# Copiar el archivo de configuración de supervisord
COPY supervisor.conf /etc/supervisor/conf.d/supervisord.conf

# Copia el certificado SSL y la clave al contenedor
COPY nginx-selfsigned.crt /etc/ssl/certs/nginx-selfsigned.crt
COPY nginx-selfsigned.key /etc/ssl/private/nginx-selfsigned.key

# Copia el archivo de configuración de Nginx
COPY  web /etc/nginx/sites-available/web

RUN ln -s /etc/nginx/sites-available/web /etc/nginx/sites-enabled/
# Expone el puerto 443 para HTTPS
EXPOSE 443
EXPOSE 80

# Comando de inicio que ejecuta Gunicorn y Nginx
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]

