# Ejercicio 1 — Despliegue de entorno Linux con Docker Compose

---

## 1. Análisis del archivo original

El archivo `docker-compose.yml` proporcionado presenta diversos errores de sintaxis y configuración que impiden su correcto funcionamiento en Docker Compose. Estos errores afectan principalmente a la estructura YAML, coherencia de nombres y configuración de servicios.

---

## 2. Errores detectados

### 2.1. Error de indentación (estructura YAML)

Las claves `networks`, `ports` y `tty` estaban mal posicionadas fuera del bloque del servicio `srv_dev_XX`.

En YAML, la indentación define la jerarquía del documento, por lo que una mala estructura provoca que Docker Compose interprete incorrectamente la configuración.

---

### 2.2. Inconsistencia en el nombre de la red

Se utilizaba `net_devXX` en el servicio, mientras que en la definición global aparecía como `net_dev_XX`.

Esto genera un error de referencia, ya que Docker no puede asociar el servicio a una red inexistente.

---

### 2.3. Imagen incorrecta

Se utilizaba la imagen `ubuntu:22`, la cual no es una etiqueta válida estándar.

Se ha corregido a `ubuntu:22.04`, versión oficial estable de Ubuntu.

---

### 2.4. Uso innecesario de puertos

Se definía el mapeo `8090:3000`, pero la imagen base de Ubuntu no ejecuta ningún servicio en ese puerto.

Esto no genera error crítico, pero supone una configuración innecesaria.

---

### 2.5. Contenedor sin proceso activo

La imagen base de Ubuntu no ejecuta procesos en primer plano, por lo que el contenedor se detenía automáticamente tras iniciarse.

---

## 3. Archivo docker-compose.yml corregido

```yaml id="finalcompose10"
version: '3.8'

services:
  srv_dev_XX:
    image: ubuntu:22.04
    container_name: srv_dev_XX
    volumes:
      - datos_devXX:/var/empresa
    networks:
      - net_dev_XX
    tty: true

volumes:
  datos_devXX:

networks:
  net_dev_XX:
```

---

## 4. Justificación técnica de los cambios

* **Corrección de indentación YAML:**
  YAML es un formato basado en espacios que define jerarquía. Una mala indentación provoca errores de interpretación en Docker Compose.

* **Unificación de nombres de red:**
  Se ha corregido el nombre para asegurar coherencia entre servicio y definición global.

* **Actualización de imagen:**
  Se utiliza `ubuntu:22.04` por ser una versión estable y soportada oficialmente.

* **Eliminación de puertos innecesarios:**
  El contenedor no expone servicios, por lo que el mapeo de puertos no es necesario.

* **Uso de `tty: true`:**
  Permite mantener el contenedor activo asignándole una terminal interactiva, evitando su finalización automática.

---

## 5. Proceso de ejecución

### 5.1. Acceso al directorio del proyecto

```bash id="step1"
cd MF0223_3_EP_01_Martret_Jaime
```

---

### 5.2. Validación del archivo

```bash id="step2"
docker-compose config
```

✔ Permite verificar la estructura del archivo antes de su ejecución.

---

### 5.3. Despliegue del entorno

```bash id="step3"
docker-compose up -d
```

---

### 5.4. Verificación de contenedores activos

```bash id="step4"
docker ps
```

Resultado esperado:

```
CONTAINER ID   IMAGE           STATUS
xxxxx          ubuntu:22.04    Up
```

---

### 5.5. Verificación de red y volumen

```bash id="step5"
docker network ls
docker volume ls
```

✔ Se confirma la creación automática de los recursos definidos.

---

### 5.6. Acceso al contenedor

```bash id="step6"
docker exec -it srv_dev_XX bash
```

---

## 6. Verificación del resultado final

* El contenedor se ejecuta correctamente en estado “Up”.
* El volumen `datos_devXX` está montado en `/var/empresa`.
* La red `net_dev_XX` se crea y asigna correctamente.
* El acceso al contenedor es funcional mediante terminal.
* No se producen errores en la ejecución del `docker-compose.yml`.

---

## 7. Conclusión

Tras la corrección del archivo original, se ha conseguido un entorno Docker funcional y coherente.

Se han aplicado buenas prácticas de Docker Compose, incluyendo:

* correcta estructuración YAML
* uso de imágenes estables
* eliminación de configuraciones innecesarias
* validación del entorno mediante comandos de verificación

El resultado es un despliegue correcto de un entorno Linux básico preparado para tareas internas en contenedor.