# Ejercicio 2 — Administración de sistemas Linux

## 1. Estructura base del proyecto

**1.1** Crear la carpeta principal:

```bash
mkdir dev_environment
```

**1.2** Crear toda la estructura de directorios en una única instrucción:

```bash
mkdir -p dev_environment/frontend/{public,src/{components,pages,styles}} dev_environment/backend/{app,config,tests} dev_environment/database/migrations dev_environment/docs dev_environment/scripts
```

---

## 2. Inicialización de archivos

**2.1** Archivos de frontend:

```bash
touch dev_environment/frontend/public/index.html
touch dev_environment/frontend/src/App.js
touch dev_environment/frontend/src/styles/main.css
```

**2.2** Archivos de backend:

```bash
touch dev_environment/backend/app/server.js
touch dev_environment/backend/config/config.json
```

**2.3** Archivos generales:

```bash
touch dev_environment/README.md
touch dev_environment/scripts/deploy.sh
```

---

## 3. Contenido

**3.1** Instalar el editor nano:

```bash
sudo apt install nano
```

**3.2** Editar los archivos con nano:

```bash
nano dev_environment/frontend/public/index.html
nano dev_environment/frontend/src/App.js
nano dev_environment/frontend/src/styles/main.css
```

---

## 4. Verificación y auditoría

**4.1** Mostrar el contenido del proyecto:

```bash
ls dev_environment/
```

**4.2** Mostrar todos los archivos (incluidos ocultos) con detalle:

```bash
ls -la dev_environment/
```

**4.3** Mostrar el contenido de `frontend/src/`:

```bash
ls dev_environment/frontend/src/
```

---

## 5. Gestión de versiones

**5.1** Crear la carpeta backup:

```bash
mkdir dev_environment/backup
```

**5.2** Copiar completamente `frontend/` dentro de `backup/`:

```bash
cp -r dev_environment/frontend/ dev_environment/backup/
```

**5.3** Copiar `server.js` como `server_backup.js` dentro de `backup/`:

```bash
cp dev_environment/backend/app/server.js dev_environment/backup/server_backup.js
```

---

## 6. Reorganización del proyecto

**6.1** Mover `main.css` a `frontend/public/`:

```bash
mv dev_environment/frontend/src/styles/main.css dev_environment/frontend/public/
```

**6.2** Renombrar `App.js` a `app.js`:

```bash
mv dev_environment/frontend/src/App.js dev_environment/frontend/src/app.js
```

**6.3** Mover `config.json` a `backend/app/`:

```bash
mv dev_environment/backend/config/config.json dev_environment/backend/app/
```

---

## 7. Permisos y seguridad

**7.1** `deploy.sh` → ejecutable solo para el propietario:

```bash
chmod 700 dev_environment/scripts/deploy.sh
```

**7.2** `server.js` → lectura/escritura propietario, solo lectura grupo:

```bash
chmod 640 dev_environment/backend/app/server.js
```

**7.3** `README.md` → solo lectura para todos:

```bash
chmod 444 dev_environment/README.md
```

Verificar los permisos:

```bash
ls -l dev_environment/scripts/deploy.sh dev_environment/backend/app/server.js dev_environment/README.md
```

---

## 8. Simulación de error y recuperación

**8.1** Eliminar `frontend/src/components/`:

```bash
rm -r dev_environment/frontend/src/components/
```

**8.2** Recuperarla desde `backup/`:

```bash
cp -r dev_environment/backup/frontend/src/components/ dev_environment/frontend/src/
```

---

## 9. Limpieza y verificación final

**1.** Eliminar la carpeta `temp/`:

```bash
rm -r dev_environment/temp/
```

**2.** Eliminar `server_backup.js`:

```bash
rm dev_environment/backup/server_backup.js
```

**3.** Mostrar la estructura completa del proyecto:

```bash
tree dev_environment/
```

**4.** Mostrar la ruta actual:

```bash
pwd
```

**5.** Mostrar el historial de comandos:

```bash
history
```

---

## 10. Informe final

Una vez completados todos los pasos del ejercicio, el entorno de desarrollo queda correctamente configurado y operativo.

### Resumen de tareas realizadas

- Se ha creado la estructura de directorios completa para un proyecto web con frontend, backend, base de datos, documentación y scripts.
- Se han inicializado todos los archivos necesarios en sus rutas correctas.
- Se han editado los archivos principales con el editor nano.
- Se ha verificado la estructura y los archivos del proyecto mediante comandos de auditoría.
- Se ha creado una copia de seguridad (backup/) del frontend y de server.js.
- Se han reorganizado archivos: main.css movido a frontend/public/, App.js renombrado a app.js y config.json movido a backend/app/.
- Se han configurado los permisos de seguridad correctamente para deploy.sh, server.js y README.md.
- Se ha simulado un error eliminando frontend/src/components/ y se ha recuperado correctamente desde el backup.
- Se ha limpiado el proyecto eliminando archivos innecesarios.

### Estado final del proyecto

El proyecto queda con una estructura limpia, organizada y con los permisos de seguridad correctamente aplicados, lista para ser utilizada por el equipo de desarrollo.
