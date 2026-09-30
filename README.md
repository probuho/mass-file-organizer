# Mass File Organizer

> Organiza, clasifica y migra archivos masivamente con verificación exhaustiva.

[![skills.sh](https://skills.sh/b/anton/mass-file-organizer)](https://skills.sh/anton/mass-file-organizer)

## Características

- **Cross-platform**: Windows, Linux, macOS
- **Verificación**: Hash, conteo, espacio en disco
- **Reversibilidad**: Mapeo origen→destino
- **Seguridad**: Respaldo comprimido antes de mover
- **Personalizable**: Categorías configurables vía JSON

## Instalación

```bash
# OpenCode
npx skills add anton/mass-file-organizer -a opencode -g

# Claude Code
npx skills add anton/mass-file-organizer -a claude-code -g

# Cursor
npx skills add anton/mass-file-organizer -a cursor -g

# Todos los agentes
npx skills add anton/mass-file-organizer --all -g
```

## Uso

```
/organize <origen> <destino>
```

Ejemplo:
```
/organize "D:\Documentos\02 MOVILNET\Edicontroller" "D:\Documentos\02 MOVILNET\APP y SER\EDI\DOCUMENTOS EDI"
```

## Categorías base

| Carpeta | Patrones |
|---------|----------|
| `01-SEGURIDAD` | Seguridad, Certificados, CER, DAT |
| `02-ACTUALIZACIONES` | Actualizacion, ACTA, DSOS |
| `03-INCIDENTES` | FALLA, falla, incidente, error |
| `04-MANUALES` | Manual, Chuletario, Instructivo |
| `05-FORMULARIOS` | FOR, Planilla, Encuesta, Matriz |
| `06-DIAGRAMAS` | diagrama, evidencia, photo |
| `07-PROCEDIMIENTOS` | PROCEDIMIENTO, script, BAT |
| `08-ACUERDOS` | ACUERDO, Acuerdo, Servicio |
| `09-CORREOS` | CORREO, MSG, eml |
| `10-TIPOS` | Tipif, Estructura, estructura |
| `11-PRESENTACIONES` | PRESENTACION, PPT, borrador |
| `12-LEGALES` | DECLARACION, COMPROMISO, AUTORIZACION |
| `13-ARCHIVOS` | OUT, TRL, ecar, log, enc, output, txt |
| `14-CODIGO_Y_STACKS` | .java, .py, .js, .ts, .sql, node_modules |

## Proceso

1. **Respaldo**: Backup comprimido del origen
2. **Inventario**: Lista de archivos antes de mover
3. **Clasificación**: Por nombre, con validación de contenido
4. **Verificación**: Conteo y hash
5. **Limpieza**: Con confirmación del usuario

## Contribuir

1. Fork el repositorio
2. Crea una rama (`git checkout -b feature/nueva-categoria`)
3. Commit (`git commit -am 'Añade nueva categoría'`)
4. Push (`git push origin feature/nueva-categoria`)
5. Abre un Pull Request

## Licencia

MIT
