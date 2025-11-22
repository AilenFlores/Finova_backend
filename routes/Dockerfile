# Imagen base con PHP 8.2 y Apache
FROM php:8.2-apache

# Instalar dependencias del sistema y extensiones de PHP necesarias para Laravel
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    libzip-dev \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    && docker-php-ext-install pdo pdo_mysql zip

# Habilitar mod_rewrite para que funcionen bien las rutas de Laravel
RUN a2enmod rewrite

# Configurar el directorio de trabajo
WORKDIR /var/www/html

# Copiar el código del proyecto al contenedor
COPY . .

# Instalar Composer (desde la imagen oficial de Composer)
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Instalar dependencias de Laravel en modo producción
RUN composer install --no-dev --optimize-autoloader

# Cambiar el DocumentRoot de Apache a la carpeta "public" de Laravel
RUN sed -ri -e 's!/var/www/html!/var/www/html/public!g' /etc/apache2/sites-available/000-default.conf

# Exponer el puerto 80 (donde escucha Apache)
EXPOSE 80

# Comando de inicio: levantar Apache en primer plano
CMD ["apache2-foreground"]
