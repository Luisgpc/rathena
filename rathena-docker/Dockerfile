FROM ubuntu:22.04

# Instalar dependencias de compilación y librerías necesarias
RUN apt-get update && apt-get install -y \
    git cmake make gcc g++ \
    mariadb-client libmysqlclient-dev zlib1g-dev libpcre3-dev \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /rathena

# Clonar tu fork de rAthena (o podrías montar el código desde fuera)
RUN git clone --depth=1 https://github.com/Luisgpc/rathena.git .

# Crear un directorio de compilación, compilar
RUN mkdir build && cd build \
    && cmake .. \
    && make -j$(nproc)

# Copiar los archivos de configuración que definiste en rathena-docker/conf/
COPY conf/ /rathena/conf/

# Exponer los puertos que usan login, char y map
EXPOSE 6900
EXPOSE 6121
EXPOSE 5121

# Comando que se ejecuta al iniciar el contenedor: lanzar todos los servidores
CMD ["sh", "-c", "./login-server & ./char-server & ./map-server && tail -f /dev/null"]
