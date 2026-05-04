# Ejercicio 3 — Diagnóstico y recuperación de rendimiento en sistema Linux

## Contexto

Tras ejecutar un script de mantenimiento en el servidor, se detectó un comportamiento anómalo. El script lanzó procesos en background que saturaron la CPU y generaron archivos temporales innecesarios en disco.

---

## 1. Análisis de CPU

**Comando utilizado:**

```bash
top -b -n 1
```

**Output relevante:**

```bash
Tasks:   6 total,   4 running,   2 sleeping,   0 stopped,   0 zombie
%Cpu(s):  7.1 us, 30.7 sy,  0.0 ni, 62.2 id
    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
     17 root      20   0    2788   1280   1280 R 100.0   0.0   0:53.36 yes
     18 root      20   0    2788   1280   1280 R 100.0   0.0   0:53.36 yes
     19 root      20   0    2788   1408   1408 R 100.0   0.0   0:53.36 yes
```

**Observación:** Los 3 procesos `yes` (PIDs 17, 18 y 19) están consumiendo cada uno el 100% de CPU, saturando completamente el sistema.

---

## 2. Análisis de procesos

**Comando utilizado:**

```bash
ps aux
```

**Output relevante:**

```bash
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root          17 95.9  0.0   2788  1280 pts/1    R    10:42   2:22 yes
root          18 95.9  0.0   2788  1280 pts/1    R    10:42   2:22 yes
root          19 95.9  0.0   2788  1408 pts/1    R    10:42   2:22 yes
```

**Procesos sospechosos identificados:** PIDs 17, 18 y 19 (proceso `yes`), lanzados en background desde la misma terminal `pts/1`, con casi el 100% de CPU cada uno.

---

## 3. Análisis de memoria

**Comando utilizado:**

```bash
free -h
```

**Output:**

```bash
               total        used        free      shared  buff/cache   available
Mem:           7.7Gi       597Mi       5.6Gi       4.0Mi       1.5Gi       6.9Gi
Swap:          2.0Gi          0B       2.0Gi
```

**Observación:** La memoria se encontraba en buen estado, con solo 597 MB usados de 7.7 GB disponibles. Los procesos que intentaban llenarla (`cat /dev/zero`) ya habían finalizado.

---

## 4. Análisis de disco

**Comandos utilizados:**

```bash
du -sh /tmp/*
df -h
```

**Output de `du -sh /tmp/*`:**

```bash
301M    /tmp/mem_test1
301M    /tmp/mem_test2
151M    /tmp/test_disk
```

**Output de `df -h`:**

```bash
Filesystem      Size  Used Avail Use% Mounted on
overlay        1007G  3.8G  952G   1% /
```

**Observación:** El script generó aproximadamente 750 MB de archivos temporales innecesarios en `/tmp/`. El disco general estaba al 1% de uso.

---

## 5. Diagnóstico

**¿Qué está causando el problema?**

El script ejecutado lanzó 3 procesos `yes` en background (con `&`) que generan salida infinita redirigida a `/dev/null`. Al no tener fin, estos procesos se quedan corriendo indefinidamente consumiendo el 100% de CPU cada uno.

**¿Qué tipo de procesos están afectando al sistema?**

- **Procesos zombie de CPU:** 3 instancias del comando `yes` (PIDs 17, 18, 19) en estado de ejecución continua que saturan la CPU.
- **Archivos basura en disco:** archivos temporales (`mem_test1`, `mem_test2`, `test_disk/`) que ocupan ~750 MB innecesariamente en `/tmp/`.

---

## 6. Solución

### 6.1 Detener los procesos problemáticos

**Comando utilizado:**

```bash
kill 17 18 19
```

Los 3 procesos `yes` fueron terminados correctamente, confirmado por el sistema:

```bash
[1]   Terminated              yes > /dev/null
[2]-  Terminated              yes > /dev/null
[3]+  Terminated              yes > /dev/null
```

### 6.2 Eliminar archivos temporales innecesarios

**Comandos utilizados:**

```bash
rm -f /tmp/mem_test1 /tmp/mem_test2
rm -rf /tmp/test_disk
```

---

## 7. Verificación

**Comandos utilizados:**

```bash
top -b -n 1
free -h
du -sh /tmp/*
```

**Resultados tras la solución:**

| Parámetro | Antes | Después |
|-----------|-------|---------|
| CPU idle | 62.2% | 100% |
| Procesos `yes` | 3 (100% CPU cada uno) | 0 |
| Archivos en `/tmp/` | ~750 MB | 0 MB (vacío) |
| Memoria usada | 597 MB | 594 MB |

**Conclusión:** El sistema volvió a la normalidad. La CPU está al 100% idle, no hay procesos anómalos en ejecución y `/tmp/` está completamente vacío.