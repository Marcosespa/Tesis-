# Proyecto de Extracción de Datos de Ciberseguridad de OAPEN

## 📋 Descripción del Proyecto

Este proyecto automatiza la extracción, clasificación y descarga de documentos académicos relacionados con ciberseguridad desde la biblioteca digital OAPEN (Open Access Publishing in European Networks). El objetivo es crear una colección organizada de PDFs académicos sobre ciberseguridad para investigación y análisis.

## 🏗️ Estructura del Proyecto

```
DatosTesis/
├── 📁 OAPEN_PDFs/
│   └── 📁 ciberseguridad/          # 168 PDFs clasificados (≈2.5GB)
├── 📄 generate_cybersecurity_json_v2.py    # Script principal de extracción
├── 📄 classify_oapen_pdfs.py               # Clasificación de PDFs
├── 📄 delete_otros_pdfs.py                 # Limpieza de archivos
├── 📄 filter_cybersecurity_items.py        # Filtrado de items
├── 📄 downlod_oapen_pdfs.py                # Descarga de PDFs
├── 📄 remove_duplicates_fast.py            # Eliminación de duplicados
├── 📄 cybersecurity_books.json             # Base de datos JSON (1.2MB)
├── 📄 cybersecurity_books_filtered.json    # Base de datos filtrada (477MB)
├── 📄 oapen_pdfs_clasificados.csv          # CSV de clasificación
└── 📄 download_progress.json               # Progreso de descargas
```

## 🔄 Flujo de Trabajo

### 1. **Extracción de Metadatos** (`generate_cybersecurity_json_v2.py`)

**Propósito**: Buscar y extraer metadatos de documentos de ciberseguridad desde la API de OAPEN.

**Características**:
- **337 términos de búsqueda** en inglés y español
- Categorías incluidas:
  - Términos generales de ciberseguridad
  - Amenazas y ataques (malware, phishing, etc.)
  - Defensas y controles (firewalls, SIEM, etc.)
  - Estándares y cumplimiento (ISO 27001, NIST, GDPR, etc.)
  - Roles y equipos (CISO, analistas, etc.)

**Proceso**:
```python
# Búsqueda por términos específicos
for term in CYBER_KEYWORDS:
    search_url = f"{OAPEN_API_BASE}/items"
    params = {
        'query': term,
        'expand': 'metadata,bitstreams',
        'limit': 100
    }
```

**Resultado**: Archivo JSON con metadatos completos de documentos encontrados.

### 2. **Filtrado de Items** (`filter_cybersecurity_items.py`)

**Propósito**: Filtrar y limpiar los metadatos extraídos para mantener solo documentos relevantes.

**Criterios de filtrado**:
- Verificación de disponibilidad de PDFs
- Validación de metadatos completos
- Eliminación de duplicados

### 3. **Clasificación de PDFs** (`classify_oapen_pdfs.py`)

**Propósito**: Clasificar automáticamente los PDFs descargados en categorías de ciberseguridad.

**Método**:
- Análisis de títulos y metadatos
- Clasificación basada en palabras clave
- Generación de CSV con clasificaciones

### 4. **Descarga de PDFs** (`downlod_oapen_pdfs.py`)

**Propósito**: Descargar los PDFs identificados desde OAPEN.

**Características**:
- Descarga masiva con control de progreso
- Manejo de errores y reintentos
- Verificación de integridad de archivos

### 5. **Limpieza y Organización** (`delete_otros_pdfs.py`)

**Propósito**: Eliminar PDFs que no pertenecen a la categoría de ciberseguridad.

**Proceso**:
- Identificación de archivos no relevantes
- Eliminación segura de archivos
- Mantenimiento de la estructura organizada

### 6. **Eliminación de Duplicados** (`remove_duplicates_fast.py`)

**Propósito**: Identificar y eliminar documentos duplicados basándose en similitud de contenido.

## 📊 Resultados Obtenidos

### Colección de PDFs
- **Total de PDFs**: 168 documentos
- **Tamaño total**: ≈2.5GB
- **Categoría**: Ciberseguridad y temas relacionados
- **Formato**: PDFs académicos de acceso abierto

### Base de Datos
- **cybersecurity_books.json**: 1.2MB (metadatos básicos)
- **cybersecurity_books_filtered.json**: 477MB (metadatos completos)
- **oapen_pdfs_clasificados.csv**: Clasificaciones detalladas

## 🛠️ Tecnologías Utilizadas

- **Python 3.13**
- **Requests**: Para llamadas a la API de OAPEN
- **JSON**: Manejo de metadatos
- **CSV**: Exportación de clasificaciones
- **API REST**: Integración con OAPEN

## 🚀 Instalación y Uso

### Requisitos Previos
```bash
# Crear entorno virtual
python3 -m venv venv
source venv/bin/activate

# Instalar dependencias
pip install requests
```

### Ejecución del Proceso Completo

1. **Extraer metadatos**:
```bash
python3 generate_cybersecurity_json_v2.py
```

2. **Filtrar items**:
```bash
python3 filter_cybersecurity_items.py
```

3. **Descargar PDFs**:
```bash
python3 downlod_oapen_pdfs.py
```

4. **Clasificar documentos**:
```bash
python3 classify_oapen_pdfs.py
```

5. **Limpiar archivos**:
```bash
python3 delete_otros_pdfs.py
```

## 📈 Palabras Clave de Búsqueda

### Inglés (131 términos)
- **General**: cyber, cybersecurity, information security, privacy
- **Amenazas**: malware, phishing, ransomware, zero-day, APT
- **Defensas**: firewall, SIEM, EDR, MFA, encryption
- **Estándares**: ISO 27001, NIST, GDPR, OWASP, MITRE ATT&CK
- **Roles**: CISO, security analyst, penetration tester

### Español (131 términos)
- **General**: ciberseguridad, seguridad informática, privacidad
- **Amenazas**: malware, phishing, ransomware, día cero, APT
- **Defensas**: cortafuegos, SIEM, EDR, autenticación multifactor
- **Estándares**: ISO 27001, marco NIST, RGPD, OWASP
- **Roles**: CISO, analista de seguridad, pentester

## 🔍 API de OAPEN

### Endpoint Principal
```
https://library.oapen.org/rest/items
```

### Parámetros de Búsqueda
- `query`: Término de búsqueda
- `expand`: metadata,bitstreams
- `limit`: Número máximo de resultados (100)

### Ejemplo de Consulta
```python
search_url = "https://library.oapen.org/rest/items"
params = {
    'query': 'cybersecurity',
    'expand': 'metadata,bitstreams',
    'limit': 100
}
```

## 📋 Características Técnicas

### Manejo de Errores
- Timeouts de 30 segundos para requests
- Reintentos automáticos en caso de fallos
- Logging detallado de errores

### Optimización de Rendimiento
- Pausas entre requests (1 segundo)
- Procesamiento por lotes
- Eliminación de duplicados eficiente

### Control de Calidad
- Verificación de integridad de archivos
- Validación de metadatos
- Clasificación automática con revisión manual

## 📊 Estadísticas del Proyecto

- **Scripts desarrollados**: 7
- **Términos de búsqueda**: 337
- **Documentos procesados**: 168 PDFs
- **Tamaño de datos**: ≈2.5GB
- **Tiempo de procesamiento**: Variable según conectividad

## 🎯 Objetivos Alcanzados

✅ **Extracción automatizada** de metadatos de OAPEN  
✅ **Búsqueda comprehensiva** con 337 términos  
✅ **Clasificación automática** de documentos  
✅ **Descarga masiva** de PDFs académicos  
✅ **Organización estructurada** de la colección  
✅ **Eliminación de duplicados** eficiente  
✅ **Documentación completa** del proceso  

## 🔮 Posibles Mejoras Futuras

- Implementación de análisis de contenido con NLP
- Clasificación automática más sofisticada
- Integración con otras fuentes académicas
- Dashboard web para exploración de la colección
- Análisis de tendencias temporales en ciberseguridad

## 📝 Detalles Técnicos de Implementación

### Script Principal: `generate_cybersecurity_json_v2.py`

**Funcionalidades principales**:
- Lista de 337 términos de búsqueda en inglés y español
- Búsqueda iterativa en la API de OAPEN
- Verificación de disponibilidad de PDFs
- Eliminación de duplicados por handle
- Extracción de metadatos completos

**Estructura de datos**:
```python
CYBER_KEYWORDS = [
    # Términos generales (inglés/español)
    "cyber", "cybersecurity", "ciberseguridad",
    # Amenazas y ataques
    "malware", "phishing", "ransomware",
    # Defensas y controles
    "firewall", "SIEM", "EDR", "MFA",
    # Estándares y cumplimiento
    "ISO 27001", "NIST", "GDPR", "OWASP",
    # Roles y equipos
    "CISO", "security analyst", "pentester"
]
```

### Proceso de Búsqueda

1. **Iteración por términos**: Cada término de búsqueda se procesa individualmente
2. **Consulta a API**: Request con parámetros de expansión de metadatos
3. **Filtrado de resultados**: Solo documentos con PDFs disponibles
4. **Deduplicación**: Eliminación de documentos ya procesados
5. **Acumulación**: Agregación de resultados únicos

### Manejo de Errores

- **Timeouts**: 30 segundos por request
- **Reintentos**: Continuación en caso de fallos
- **Logging**: Información detallada de cada operación
- **Pausas**: 1 segundo entre requests para no sobrecargar la API

## 🗂️ Organización de Archivos

### Estructura de Directorios
```
OAPEN_PDFs/
└── ciberseguridad/
    ├── 9781439811658.pdf (12MB)
    ├── 9781040306987.pdf (272MB)
    ├── 1006885.pdf (8.9MB)
    └── ... (168 archivos total)
```

### Archivos de Datos
- **cybersecurity_books.json**: Metadatos básicos (1.2MB)
- **cybersecurity_books_filtered.json**: Metadatos completos (477MB)
- **oapen_pdfs_clasificados.csv**: Clasificaciones por categoría
- **download_progress.json**: Estado de descargas

## 🔧 Configuración del Entorno

### Dependencias
```bash
pip install requests
```

### Variables de Configuración
```python
OAPEN_API_BASE = "https://library.oapen.org/rest"
OUTPUT_JSON = "cybersecurity_books_complete.json"
```

### Parámetros de Búsqueda
```python
params = {
    'query': term,
    'expand': 'metadata,bitstreams',
    'limit': 100
}
```

## 📈 Métricas de Rendimiento

### Tiempo de Procesamiento
- **Búsqueda por término**: ~1-2 segundos
- **Total de términos**: 337
- **Tiempo estimado**: 5-10 minutos
- **Pausas entre requests**: 1 segundo

### Uso de Recursos
- **Memoria**: Variable según tamaño de resultados
- **Almacenamiento**: ~2.5GB para PDFs + 478MB para metadatos
- **Red**: ~500MB de descarga de metadatos

## 🎓 Aplicaciones Académicas

### Investigación en Ciberseguridad
- Análisis de tendencias en publicaciones académicas
- Estudio de evolución de amenazas cibernéticas
- Revisión de estándares y marcos de trabajo

### Minería de Datos
- Extracción de patrones en títulos y abstracts
- Análisis de coautoría y colaboraciones
- Identificación de temas emergentes

### Bibliometría
- Análisis de impacto de publicaciones
- Mapeo de redes de investigación
- Identificación de líderes en el campo

---

**Desarrollado para investigación académica en ciberseguridad**  
**Fuente de datos**: OAPEN (Open Access Publishing in European Networks)  
**Última actualización**: Septiembre 2024


## Estructura del proyecto (RAG)

```
tu-proyecto/
├── README.md
├── .gitignore
├── pyproject.toml                # o requirements.txt
├── docker-compose.yml            # Weaviate
├── .env.example                  # sin claves
├── configs/
│   ├── rag.yaml                  # chunking/retrieval/rerank
│   └── weaviate.schema.json      # clase BookChunk
├── data/
│   ├── pdfs/                     # PDFs fuente (múltiples orígenes)
│   │   ├── OAPEN_PDFs/           # PDFs de OAPEN
│   │   ├── USENIX/               # PDFs de USENIX (pendiente)
│   │   └── NIST/                 # PDFs de NIST (pendiente)
│   ├── interim/                  # texto por página, limpio (jsonl)
│   ├── chunks/                   # *.pages.jsonl, *.chunks.jsonl, all_chunks.jsonl
│   └── models/                   # (opcional) cache de modelos HF
├── src/
│   ├── ingest/
│   │   └── extract_pdf.py        # PyMuPDF + OCR + limpieza
│   ├── process/
│   │   ├── chunking.py           # jerárquico + semántico (400 tok + 15%)
│   │   └── quality.py            # banderas de calidad (ocr, vacío, etc.)
│   ├── index/
│   │   ├── embeddings.py         # SentenceTransformers (CPU/GPU)
│   │   ├── weaviate_client.py    # helpers (crear clase, batch upsert, query)
│   │   └── ingest_to_weaviate.py # lee all_chunks.jsonl → indexa
│   ├── api/
│   │   ├── server.py             # FastAPI: /query (híbrida) + /citations
│   │   └── retriever.py          # híbrida + (opcional) rerank local
│   └── eval/
│       ├── build_eval_set.py
│       └── evaluate_rag.py
├── scripts/
│   ├── up_weaviate.sh
│   ├── 10_extract.sh
│   ├── 20_chunk.sh
│   ├── 30_index.sh
│   └── 40_query_examples.sh
└── tests/
    ├── test_chunking.py
    └── test_weaviate.py
```

### Nota sobre ScriptsData
Lo previo en `ScriptsData/` (descarga y clasificación) se mantiene como etapa de adquisición. El pipeline nuevo opera sobre PDFs ya disponibles en `data/raw/` (puedes organizar por fuente como `data/raw/OAPEN_PDFs/`, `data/raw/USENIX/`, `data/raw/NIST/`), genera `interim/*.pages.jsonl`, produce chunks en `data/chunks/*.chunks.jsonl` y consolida en `data/chunks/all_chunks.jsonl`, listo para indexar en Weaviate.

### Cómo correr
```bash
# 1) levantar Weaviate
bash scripts/rag/up_weaviate.sh

# 2) extraer texto por página (limpio)
# puedes apuntar a cualquier subcarpeta de data/raw/
bash scripts/rag/10_extract.sh data/raw

# 3) generar chunks jerárquicos + semánticos
bash scripts/rag/20_chunk.sh

# 4) indexar en Weaviate
bash scripts/rag/30_index.sh
```

## Nueva estructura unificada (RAG + FT)

```
DatosTesis/
├── README.md
├── .gitignore
├── .env.example
├── docker-compose.yml
├── requeriments.txt
├── Makefile                      # (opcional) targets: rag-extract, rag-chunk, rag-index, ft-prepare, ft-train
│
├── configs/
│   ├── rag.yaml
│   ├── weaviate.schema.json
│   └── ft.yaml                   # NUEVO (hp de FT: modelo base, LoRA, lr, etc.)
│
├── data/                         # *Data lake compartido*
│   ├── raw/                      # PDFs originales (lo que hoy tienes en data/pdfs/*)
│   │   ├── USENIX/
│   │   ├── NIST/
│   │   └── OAPEN_PDFs/...
│   ├── interim/                  # páginas limpias por libro (*.pages.jsonl)
│   ├── clean/                    # (si guardas normalizaciones per-page)
│   ├── chunks/                   # *.chunks.jsonl y all_chunks.jsonl (para RAG)
│   ├── export/                   # exports varios (ej. CSV/Parquet)
│   ├── models/                   # (opcional) caché de modelos locales HF
│   ├── ft_raw/                   # NUEVO: materiales base para FT (no-chunks)
│   └── ft_datasets/              # NUEVO: datasets finales FT (train/val/test JSONL)
│
├── src/
│   ├── common/                   # NUEVO: utilidades compartidas por RAG y FT
│   │   ├── io.py                 # leer/escribir jsonl, paths, hashing source_id
│   │   ├── textutils.py          # normalización, sent-split
│   │   ├── licenses.py           # (opcional) control de licencias/flags
│   │   └── evalutils.py
│   │
│   ├── rag/                      # (mueve aquí tu código RAG)
│   │   ├── ingest/
│   │   │   └── extract_pdf.py    # (lo que tienes) PyMuPDF + OCR + limpieza
│   │   ├── process/
│   │   │   ├── chunking.py       # (tu actual) jerárquico + semántico
│   │   │   └── quality.py
│   │   ├── index/
│   │   │   ├── embeddings.py     # sentence-transformers (local)
│   │   │   ├── ingest_to_weaviate.py
│   │   │   └── weaviate_client.py
│   │   ├── api/
│   │   │   ├── retriever.py
│   │   │   └── server.py
│   │   └── eval/
│   │       ├── build_eval_set.py
│   │       └── evaluate_rag.py
│   │
│   └── ft/                       # NUEVO: fine-tuning separado
│       ├── prepare_dataset.py    # crea ejemplos (prompt→respuesta, extracción, etc.)
│       ├── train_lora.py         # HF Transformers + PEFT/QLoRA (sin OpenAI)
│       ├── infer.py              # inferencia del checkpoint
│       └── eval_ft.py            # métricas EM/F1/ROUGE según tarea
│
├── scripts/
│   ├── rag/                      # (mueve aquí los tuyos)
│   │   ├── up_weaviate.sh
│   │   ├── 10_extract.sh
│   │   ├── 20_chunk.sh
│   │   ├── 30_index.sh
│   │   └── 40_query_examples.sh
│   └── ft/
│       ├── 10_prepare.sh         # genera ft_datasets/{train,val}.jsonl
│       ├── 20_train.sh           # lanza FT (LoRA/QLoRA)
│       └── 30_eval.sh
│
└── tests/
    ├── test_chunking.py
    ├── test_weaviate.py
    └── test_ft_dataset.py        # NUEVO: valida formato FT (mensajes, campos, tamaños)
```
