# Nominatim con PostGIS en Docker

Este proyecto proporciona un entorno basado en Docker para ejecutar **Nominatim**, un servicio de geocodificación basado en **OpenStreetMap**, junto con **PostGIS** como base de datos.

## Servicios

### 1. **PostGIS**

- Imagen: `postgis/postgis:15-3.3`
- Expone el puerto `5432`
- Configurado con usuario, contraseña y base de datos `nominatim`
- Verifica la disponibilidad con `pg_isready`

### 2. **Nominatim**

- Imagen: `mediagis/nominatim:4.1`
- Depende de PostGIS y espera a que esté listo antes de iniciar
- Descarga y procesa el archivo PBF de México desde Geofabrik
- Expone el puerto `8080`
- Verifica la disponibilidad con `curl`

### 3. **db_initializer**

- Imagen: `postgis/postgis:15-3.3`
- Espera a que PostgreSQL esté disponible antes de crear la base de datos `nominatim`

## Requisitos previos

- **Docker** y **Docker Compose** instalados en el sistema
- Conexión a internet para descargar las imágenes necesarias y el archivo PBF

## Instrucciones de uso

### 1. Clonar el repositorio
```bash
 git clone <URL_DEL_REPOSITORIO>
 cd <NOMBRE_DEL_REPOSITORIO>
```

### 2. Iniciar los contenedores
```bash
 docker-compose up -d
```
Esto iniciará los servicios en segundo plano.

### 3. Verificar los logs
```bash
 docker-compose logs -f
```
Para asegurarse de que los servicios se están ejecutando correctamente.

### 4. Acceder a Nominatim
Abrir en el navegador:
```
http://localhost:8080
```

### 5. Detener los contenedores
```bash
 docker-compose down
```

## Personalización

### Cambiar el área de descarga
El servicio Nominatim está configurado para descargar los datos de México. Para usar otra región, modifica la variable `PBF_URL` en `docker-compose.yml`:
```yaml
PBF_URL: "https://download.geofabrik.de/europe/germany-latest.osm.pbf"
```

### Persistencia de datos
Si deseas que los datos de la base de datos sean persistentes, agrega un volumen en el servicio `postgis`:
```yaml
volumes:
  - postgis_data:/var/lib/postgresql/data
```
Y agrégalo en la parte inferior del archivo:
```yaml
volumes:
  postgis_data:
```

## Contribuir
Si deseas mejorar este proyecto, ¡cualquier contribución es bienvenida!

## Licencia
Este proyecto se distribuye bajo la licencia **MIT**.
