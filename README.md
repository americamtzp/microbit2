

# 📊 Estación Meteorológica con Grafana e InfluxDB

## 🧱 Requisitos Previos

* Una instancia EC2 con Ubuntu.
* Docker y Docker Compose instalados.
* InfluxDB y Grafana ejecutándose en contenedores.


---

## ⚙️ Paso 1: Iniciar InfluxDB y Grafana con Docker

### `docker-compose.yml` básico:

```yaml
version: '3'

services:
  influxdb:
    image: influxdb:1.8
    ports:
      - "8086:8086"
    volumes:
      - influxdb:/var/lib/influxdb

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
    depends_on:
      - influxdb

volumes:
  influxdb:
```

### Comando para iniciar los servicios:

```bash
docker-compose up -d
```

---

## 🗃️ Paso 2: Crear base de datos en InfluxDB

### Ingresar al contenedor de InfluxDB:

```bash
docker exec -it <nombre_del_contenedor_influxdb> influx
```

### Crear la base de datos:

```sql
CREATE DATABASE sensores;
```

### Verificar que fue creada:

```sql
SHOW DATABASES;
```

### Seleccionar la base de datos (¡sin la palabra `DATABASE`!):

```sql
USE sensores;
```

---

## 🌡️ Paso 3: Insertar y Consultar Datos

### Insertar datos simulados:

```sql
INSERT estacion_meteorologica_simulada,ubicacion=laboratorio temperatura=25.0,luz=70.0
```

### Verificar los datos:

```sql
SHOW MEASUREMENTS;
SELECT * FROM estacion_meteorologica_simulada;
```

---

## 📈 Paso 4: Conectar Grafana

1. Accede a `http://localhost:3000` (usuario y contraseña por defecto: `admin`).
2. Ve a **Configuration > Data Sources**.
3. Añade una fuente de datos **InfluxDB**.
4. Configura:

   * **URL:** `http://influxdb:8086`
   * **Database:** `sensores`
   * **Access:** `Server`
   * **Query Language:** `InfluxQL`
   * Guarda y prueba.

---

## 🧪 Paso 5: Crear Panel en Grafana

1. Ve a un nuevo Dashboard.
2. Agrega un panel.
3. En la sección de Query, escribe:

```sql
SELECT "temperatura" FROM "estacion_meteorologica_simulada" WHERE $timeFilter
```

✅ **Nota:** Asegúrate de usar `$timeFilter` (sin guion bajo) si usas Grafana moderno. Si aparece un error como `missing parameter: __timeFilter`, significa que se usó mal el marcador de tiempo.

---




---

## Resultados en Grafana: 


![image](https://github.com/user-attachments/assets/f8986f5b-6bf3-44e3-bded-bb54622bf976)

![image](https://github.com/user-attachments/assets/43058fd0-ca8c-4510-9621-bccc05db1439)

![image](https://github.com/user-attachments/assets/636e0eae-39f1-4613-8780-b11a90a52252)
