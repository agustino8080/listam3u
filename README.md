# 📺 IPTV M3U - Listas de Canales con Mantenimiento Automático

[![Limpieza de Archivos M3U](https://github.com/user/repo/actions/workflows/clean-lists.yml/badge.svg)](https://github.com/user/repo/actions/workflows/clean-lists.yml)
[![Verificar y Limpiar Canales](https://github.com/user/repo/actions/workflows/verify-channels.yml/badge.svg)](https://github.com/user/repo/actions/workflows/verify-channels.yml)

## 📋 Descripción

Este repositorio contiene listas de reproducción M3U para IPTV con **mantenimiento automático diario**. El sistema verifica y limpia automáticamente los canales offline y archivos temporales todos los días a las **09:00 AM UTC**.

## ⚙️ Agentes de Mantenimiento

### 1. 🧹 Limpieza de Archivos M3U (`clean-lists.yml`)

**Horario:** Diario a las 09:00 AM

**Funciones:**
- ✅ Detecta y elimina archivos temporales (`*-temp-*`, `temp-*`, `*.tmp`)
- ✅ Elimina archivos M3U antiguos (más de 30 días sin modificar)
- ✅ Genera reportes detallados de limpieza
- ✅ Realiza commit automático de cambios
- ✅ Sube artifacts con reportes de ejecución

**Criterios de eliminación:**
- Archivos con patrones: `*-temp-*`, `temp-*`, `*.tmp`
- Archivos `.m3u` y `.m3u8` sin modificaciones en +30 días

---

### 2. 🔍 Verificar y Limpiar Canales (`verify-channels.yml`)

**Horario:** Diario a las 09:00 AM

**Funciones:**
- ✅ Escanea todos los archivos M3U del repositorio
- ✅ Verifica cada canal individualmente (HTTP HEAD/GET requests)
- ✅ Elimina canales offline de las listas
- ✅ Guarda reportes de canales eliminados
- ✅ Actualiza archivos manteniendo solo canales online
- ✅ Genera estadísticas detalladas por archivo y globales
- ✅ Realiza commit automático con resumen de cambios

**Proceso de verificación:**
1. Parsea cada archivo M3U extrayendo nombre y URL de cada canal
2. Testea conectividad con timeout de 10 segundos
3. Clasifica canales como Online/Offline
4. Reescribe el archivo solo con canales funcionales
5. Genera reporte de canales eliminados

---

## 📅 Programación

Ambos workflows se ejecutan automáticamente:

| Workflow | Horario | Frecuencia |
|----------|---------|------------|
| Limpieza de Archivos | 09:00 AM UTC | Diario |
| Verificación de Canales | 09:00 AM UTC | Diario |

**Nota:** También pueden ejecutarse manualmente desde la pestaña "Actions" de GitHub.

---

## 📁 Estructura del Repositorio

```
/workspace/
├── .github/
│   └── workflows/
│       ├── clean-lists.yml      # Limpieza automática
│       └── verify-channels.yml  # Verificación de canales
├── *.m3u                        # Listas de canales
├── *.m3u8                       # Listas alternativas
├── reports/                     # Reportes generados (auto)
└── README.md                    # Este archivo
```

---

## 🚀 Ejecución Manual

Puedes ejecutar los workflows manualmente:

1. Ve a la pestaña **Actions** en GitHub
2. Selecciona el workflow deseado:
   - "Limpieza de Archivos M3U"
   - "Verificar y Limpiar Canales"
3. Click en **"Run workflow"**
4. Selecciona la rama (default: main/master)
5. Click en **"Run workflow"**

---

## 📊 Reportes

Cada ejecución genera:

### Reportes de Limpieza
- Lista de archivos temporales eliminados
- Lista de archivos antiguos eliminados
- Estadísticas de espacio liberado

### Reportes de Verificación
- Total de canales verificados
- Porcentaje de canales online/offline
- Lista detallada de canales eliminados
- URLs de canales no funcionales

Los reportes están disponibles como **artifacts** en cada run de GitHub Actions (retención: 30 días).

---

## 🔧 Configuración Técnica

### Dependencias
- Python 3.11+
- Librería `requests` para verificación HTTP

### Permisos Requeridos
El workflow necesita permisos para:
- Leer contenido del repositorio
- Escribir commits
- Subir artifacts

### Timeouts
- Verificación de canales: 60 minutos máximo
- Timeout por canal: 10 segundos

---

## 📈 Métricas

Después de cada ejecución, se muestran métricas en el summary:

| Métrica | Descripción |
|---------|-------------|
| Archivos procesados | Cantidad de archivos M3U escaneados |
| Total de canales | Número total de canales encontrados |
| Canales online | Canales verificados como funcionales |
| Canales offline | Canales eliminados por no funcionar |
| Tasa de éxito | Porcentaje de canales online |

---

## 🛡️ Seguridad

- ✅ No se almacenan credenciales
- ✅ Todos los streams son públicos
- ✅ Los reportes no incluyen información sensible
- ✅ Ejecución en entorno aislado de GitHub Actions

---

## 📝 Notas Importantes

1. **Horario UTC:** Los workflows están configurados en tiempo UTC (09:00 AM UTC)
2. **Timeout:** La verificación puede tardar según la cantidad de canales
3. **Backups:** Se recomienda mantener copias locales de las listas originales
4. **Frecuencia:** La verificación diaria asegura listas actualizadas

---

## 🤝 Contribución

Para agregar nuevas listas M3U:
1. Sube el archivo `.m3u` o `.m3u8` al repositorio
2. El sistema lo detectará automáticamente en la próxima ejecución
3. Los canales serán verificados y mantenidos automáticamente

---

## 📞 Soporte

Para issues relacionados con:
- Canales offline → Se eliminan automáticamente
- Errores en workflows → Revisar logs en GitHub Actions
- Nuevas funcionalidades → Abrir issue en el repositorio

---

## 📄 Licencia

Este proyecto es para uso personal y educativo. Asegúrate de tener derechos para distribuir las listas de canales utilizadas.

---

**Última actualización:** $(date +%Y-%m-%d)
**Mantenimiento:** Automático diario a las 09:00 AM UTC
