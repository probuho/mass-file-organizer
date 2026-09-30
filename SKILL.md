---
name: mass-file-organizer
description: Organiza, clasifica y migra archivos masivamente con verificación exhaustiva. Cross-platform: Windows, Linux, macOS.
---

# Mass File Organizer

> Organización masiva de archivos con verificación exhaustiva y reversibilidad.

## Cuándo usar

- Tienes miles de archivos desorganizados en un directorio
- Necesitas clasificarlos por tipo, nombre o contenido
- Quieres migrar documentación de un lugar a otro de forma segura
- Necesitas un proceso repetible y auditable

## Parámetros

```
/organize <origen> <destino> [--backup-ruta <ruta>] [--categorias <archivo>]
```

- `origen`: Ruta del directorio con archivos desorganizados
- `destino`: Ruta del directorio donde se clasificarán
- `--backup-ruta`: Ruta para respaldo (preguntar al usuario si no se especifica)
- `--categorias`: Archivo JSON con categorías personalizadas (opcional)

## Pre-validación

Antes de ejecutar, preguntar al usuario:

1. **Sistema operativo**: ¿Windows, Linux o macOS?
2. **Ruta de respaldo**: ¿Usar predeterminada o especificar?
   - Windows: `C:\Users\<usuario>\Downloads\Organizer_Backup`
   - Linux: `/home/<usuario>/Downloads/Organizer_Backup`
   - macOS: `/Users/<usuario>/Downloads/Organizer_Backup`
3. **Espacio en disco**: Validar que haya suficiente espacio (2x el tamaño del origen)

## Categorías base (personalizables)

Estas son categorías sugeridas. El usuario puede modificarlas o proporcionar un archivo JSON:

| Carpeta | Patrones sugeridos |
|---------|-------------------|
| `01-SEGURIDAD` | Seguridad, Certificados, CER, DAT, IMP, acceso |
| `02-ACTUALIZACIONES` | Actualizacion, ACTA, DSOS, 2018, 2019, upgrade |
| `03-INCIDENTES` | FALLA, falla, incidente, error, bug |
| `04-MANUALES` | Manual, Chuletario, Instructivo, HTML, HTM, Soporte |
| `05-FORMULARIOS` | FOR, Planilla, Encuesta, Matriz, XLS, XLSX, XLSM |
| `06-DIAGRAMAS` | diagrama, Diagrama, evidencia, photo, PNG, JPG, BMP |
| `07-PROCEDIMIENTOS` | PROCEDIMIENTO, script, BAT, AJRC, FTP, PASOS |
| `08-ACUERDOS` | ACUERDO, Acuerdo, Servicio, contrato |
| `09-CORREOS` | CORREO, MSG, eml, HD0000, RESUMEN |
| `10-TIPOS` | Tipif, Estructura, estructura, Estructuras |
| `11-PRESENTACIONES` | PRESENTACION, PPT, ppt, borrador |
| `12-LEGALES` | DECLARACION, COMPROMISO, AUTORIZACION, RTF, XPS |
| `13-ARCHIVOS` | OUT, TRL, ecar, log, enc, output, txt, TXT |
| `14-CODIGO_Y_STACKS` | .java, .py, .js, .ts, .cpp, .sql, node_modules, venv, .git |

## Proceso (5 pasos)

### Paso 1: Respaldo comprimido

**Windows (PowerShell)**:
```powershell
$timestamp = Get-Date -Format "yyyyMMdd_HHmmss"
$backupPath = "$backupRuta\${nombreOrigen}_${timestamp}"
Copy-Item -Path $origen -Destination $backupPath -Recurse -Force
Compress-Archive -Path $backupPath -DestinationPath "${backupPath}.zip" -CompressionLevel Optimal
Remove-Item -Path $backupPath -Recurse -Force
```

**Linux/macOS (bash)**:
```bash
timestamp=$(date +%Y%m%d_%H%M%S)
backup_path="${backup_ruta}/${nombre_origen}_${timestamp}"
cp -r "$origen" "$backup_path"
tar -czf "${backup_path}.tar.gz" -C "$backup_ruta" "${nombre_origen}_${timestamp}"
rm -rf "$backup_path"
```

### Paso 2: Inventario

Crear inventario antes de mover:
```bash
# Linux/macOS
find "$origen" -type f > "${destino}/inventario_${timestamp}.txt"
```
```powershell
# Windows
Get-ChildItem $origen -Recurse -File | Select-Object FullName, Length, Extension | Export-Csv -Path "$destino\inventario_$timestamp.csv" -Encoding UTF8
```

### Paso 3: Clasificación

**Reglas**:
- Clasificación primaria por nombre (rápida)
- Si el nombre es ambiguo, validar contenido
- Código fuente y stacks van a `14-CODIGO_Y_STACKS` (no tocar)
- Imágenes: clasificar solo por nombre
- Duplicados: renombrar con timestamp

### Paso 4: Verificación

Contar archivos antes y después. Verificar que origen quede vacío.

### Paso 5: Limpieza con confirmación

Preguntar al usuario antes de eliminar backup o carpetas vacías.

## Log de operaciones

**Windows (PowerShell)**:
```powershell
Start-Transcript -Path "$destino\organizacion_$timestamp.log"
# ... operaciones ...
Stop-Transcript
```

**Linux/macOS (bash)**:
```bash
exec > >(tee -a "${destino}/organizacion_${timestamp}.log") 2>&1
# ... operaciones ...
```

## Reversibilidad

Crear archivo de mapeo origen→destino:
```powershell
# Windows
$movidos | Export-Csv -Path "$destino\mapeo_$timestamp.csv" -Encoding UTF8
```
```bash
# Linux/macOS
echo "$origen -> $destino" >> "${destino}/mapeo_${timestamp}.txt"
```

## Manejo de edge cases

| Caso | Solución |
|------|----------|
| Emoji en nombres | Reemplazar con `PAGO_` o timestamp |
| Caracteres especiales | Usar herramientas nativas del SO |
| Archivos en uso | Reportar y continuar |
| Duplicados | Renombrar con timestamp |
| Código fuente | `14-CODIGO_Y_STACKS` (no tocar) |
| Stacks/dependencias | `14-CODIGO_Y_STACKS` (no tocar) |
| Imágenes | Clasificar solo por nombre |
| Archivos corruptos | Hash verification + reportar |

## Verificación final

- [ ] Origen vacío (0 archivos)
- [ ] Destino completo
- [ ] Backup verificado
- [ ] Espacio validado
- [ ] Sin errores
- [ ] Log generado
- [ ] Mapeo creado
- [ ] Usuario confirmó limpieza

## Ejemplo de categorías personalizadas (JSON)

```json
{
  "categorias": {
    "01-SEGURIDAD": ["Seguridad", "Certificados", "CER", "DAT"],
    "02-ACTUALIZACIONES": ["Actualizacion", "ACTA", "DSOS"],
    "03-INCIDENTES": ["FALLA", "falla", "incidente", "error"],
    "04-MANUALES": ["Manual", "Chuletario", "Instructivo"],
    "05-FORMULARIOS": ["FOR", "Planilla", "Encuesta", "Matriz"],
    "06-DIAGRAMAS": ["diagrama", "evidencia", "photo"],
    "07-PROCEDIMIENTOS": ["PROCEDIMIENTO", "script", "BAT"],
    "08-ACUERDOS": ["ACUERDO", "Acuerdo", "Servicio"],
    "09-CORREOS": ["CORREO", "MSG", "eml"],
    "10-TIPOS": ["Tipif", "Estructura", "estructura"],
    "11-PRESENTACIONES": ["PRESENTACION", "PPT", "borrador"],
    "12-LEGALES": ["DECLARACION", "COMPROMISO", "AUTORIZACION"],
    "13-ARCHIVOS": ["OUT", "TRL", "ecar", "log", "enc", "output", "txt"],
    "14-CODIGO_Y_STACKS": [".java", ".py", ".js", ".ts", ".cpp", ".sql", "node_modules", "venv", ".git"]
  }
}
```
