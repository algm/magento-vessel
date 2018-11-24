# Docker para magento2

Configuración para montar magento2 con docker (incluye un magento 2.2 limpio para poder probar) .

Esta configuración está basada en [vessel](https://vessel.shippingdocker.com/) y adaptada a Magento.

Se incluye un script `vessel` para ejecutar comandos comunes fácilmente sin necesidad de pasar parámetros a los comandos de docker.

## Uso

### Instalación

Construir los contenedores.

```bash
./vessel build
```

Arrancar el sistema

```bash
./vessel start
```

La aplicación será accesible desde http://localhost

### Ejecutar el cli de magento

Se ha incluido un parámetro en el script vessel para ejecutar comandos de magento directamente:

```bash
./vessel mage comando
```

Ejemplo:

```bash
./vessel mage cron:run
```

### Ejecutar comandos de composer

Se incluye un parámetro `composer` (o `comp`) en el script vessel para ejecutar directamente comandos de composer.

Ejemplo:

```bash
./vessel composer require illuminate/support
```

### Parar el sistema

```bash
./vessel stop
```
