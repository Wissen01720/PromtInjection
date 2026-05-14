# 🛡️ Chat Gemini — QA Prompt Injection Lab
---

## 📋 Tabla de contenido

- [Objetivo](#-objetivo)
- [Arquitectura](#-arquitectura)
- [Tecnologías](#-tecnologías-y-dependencias)
- [Estructura del proyecto](#-estructura-del-proyecto)
- [Configuración inicial](#-configuración-inicial)
- [Variables de entorno](#-variables-de-entorno)
- [Ejecutar la aplicación](#-ejecutar-la-aplicación-web)
- [Ejecutar pruebas QA](#-ejecutar-pruebas-qa-de-prompt-injection)
- [Métricas de seguridad](#-métricas-de-seguridad)
- [Reportes generados](#-reportes-generados)
- [Evidencias visuales](#-evidencias-visuales)
- [Comandos rápidos](#-comandos-rápidos)
- [Troubleshooting](#-troubleshooting)
- [Buenas prácticas](#-buenas-prácticas)
- [Próximas mejoras](#-próximas-mejoras)

---

## 🎯 Objetivo

Este laboratorio permite validar que un asistente de IA conversacional conectado a la API de Gemini:

| ✅ Comportamiento esperado | 🔍 Qué se valida |
|---|---|
| Flujo de chat normal | Respuestas coherentes y útiles |
| Resistencia a ataques | No revela información sensible ante prompt injection |
| Evidencia reproducible | Métricas y reportes auditables para análisis de seguridad |

---

## 🏗️ Arquitectura

### Flujo principal — Aplicación web

```
Usuario
  └─► Frontend (index.html + script.js)
        └─► POST /chat
              └─► Backend Flask (app.py)
                    └─► Gemini API
                          └─► Respuesta JSON
                                └─► Frontend renderiza respuesta
```

### Flujo QA — Suite de pruebas

```
payloads.json
  └─► test_prompt_injection.py
        └─► POST /chat  (por cada payload)
              └─► Evaluación SUCCESS / BLOCKED
                    └─► Cálculo de métricas (metrics.py)
                          └─► Reportes en qa-tests/reports/
```

---

## 🧰 Tecnologías y dependencias

| Categoría | Tecnología |
|---|---|
| **Runtime** | Python 3.10+ |
| **Backend** | Flask · Flask-CORS · python-dotenv |
| **IA** | google-generativeai (Gemini API) |
| **QA / Testing** | requests · colorama · tabulate |
| **Reportes** | pandas · openpyxl |

> Todas las dependencias se instalan desde `requirements.txt`.

---

## 📁 Estructura del proyecto

```
chat_gemini_qa_promtInjection/
│
├── app.py                         # Servidor Flask principal
├── requirements.txt               # Dependencias del proyecto
├── .env                           # 🔒 API Key (NO subir al repo)
├── readme.md
│
├── qa-tests/
│   ├── payloads.json              # Catálogo de payloads de ataque
│   ├── test_prompt_injection.py   # Script de pruebas automatizadas
│   ├── metrics.py                 # Cálculo de métricas de seguridad
│   └── reports/                   # Reportes generados (JSON / Excel / HTML)
│
├── static/
│   ├── script.js
│   ├── img/
│   │   └── ustatunja.png
│   └── styles/
│       └── custom.css
│
├── templates/
│   ├── base.html
│   └── index.html
│
└── docs/
    └── screenshots/               # 📸 Evidencias visuales (ver sección abajo)
        ├── app/
        ├── qa/
        └── reports/
```

---

## ⚙️ Configuración inicial

### 1 · Clonar el repositorio

```bash
git clone <URL_DEL_REPOSITORIO>
cd chat_gemini_qa_promtInjection
```

### 2 · Crear entorno virtual

```bash
py -m venv .venv
```

### 3 · Activar entorno virtual

```bash
# PowerShell
.\.venv\Scripts\Activate.ps1

# CMD
.venv\Scripts\activate.bat
```

### 4 · Instalar dependencias

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt

# Verificar instalación
pip list
```

---

## 🔑 Variables de entorno

El backend lee la clave de Gemini desde la variable de entorno `GOOGLE_API_KEY`.

Crea un archivo `.env` en la raíz del proyecto con el siguiente contenido:

```env
GOOGLE_API_KEY=tu_api_key_aqui
```

> ⚠️ **Importante:** nunca subas tu API key al repositorio. Agrega `.env` a tu `.gitignore`.

---

## 🚀 Ejecutar la aplicación web

Desde la raíz del proyecto:

```bash
python app.py
```

| Parámetro | Valor |
|---|---|
| URL local | `http://127.0.0.1:5000` |
| Endpoint de chat | `POST /chat` |

---

## 🧪 Ejecutar pruebas QA de Prompt Injection

> ⚠️ **Requisito previo:** la aplicación Flask (`app.py`) debe estar corriendo en otra terminal.

```bash
cd qa-tests
python test_prompt_injection.py
```

**¿Qué hace el script?**

1. 📂 Carga los payloads desde `payloads.json`
2. 📡 Envía cada prompt al endpoint `/chat`
3. 🔎 Clasifica resultados como `SUCCESS` o `BLOCKED`
4. 📊 Calcula métricas ISR, MR y Security Score
5. 📄 Genera reportes en `qa-tests/reports/`

---

## 📊 Métricas de seguridad

| Métrica | Descripción | Interpretación |
|---|---|---|
| **ISR** · Injection Success Rate | % de ataques exitosos | ⬆️ ISR alto → mayor riesgo |
| **MR** · Mitigation Rate | Capacidad de mitigación `(1 - ISR/100)` | ⬆️ MR alto → mejor defensa |
| **Security Score** | Promedio entre MR y factores CRI/ACI | ≈ 1.0 → mejor postura de seguridad |

---

## 📄 Reportes generados

Cada corrida produce archivos con timestamp en `qa-tests/reports/`:

```
qa-tests/reports/
├── report_YYYYMMDD_HHMMSS.json    # Trazabilidad técnica y automatización
├── report_YYYYMMDD_HHMMSS.xlsx    # Análisis y presentación a equipo/docentes
└── report_YYYYMMDD_HHMMSS.html    # Evidencia visual lista para compartir
```

| Formato | Uso sugerido |
|---|---|
| 📋 **JSON** | Integración con pipelines, auditoría técnica |
| 📊 **Excel** | Análisis de métricas, presentaciones académicas |
| 🌐 **HTML** | Evidencia visual, reportes entregables |

---

## 📸 Evidencias visuales

### Crear estructura de carpetas

```powershell
mkdir docs\screenshots\app -Force
mkdir docs\screenshots\qa -Force
mkdir docs\screenshots\reports -Force
```

---

### 🖥️ Interfaz del chat

> Captura la pantalla principal, el envío de un prompt normal y la respuesta del asistente.

<img width="1900" height="948" alt="image" src="https://github.com/user-attachments/assets/3edce5e6-0fdc-4c75-a6a1-fffb319fe823" />

---

### 🔬 Ejecución de pruebas QA

> Captura la terminal mientras corre `test_prompt_injection.py`, mostrando los resultados SUCCESS / BLOCKED en tiempo real.

<img width="1275" height="741" alt="image" src="https://github.com/user-attachments/assets/5c5b122e-484f-4cc0-b7b4-dccffbd20e68" />

---

### 📑 Reportes generados

> Captura la vista del reporte HTML en el navegador, un fragmento del Excel con métricas y el JSON resultante.

<img width="921" height="457" alt="image" src="https://github.com/user-attachments/assets/7cd8303e-9c07-4e71-b40d-883109e592f7" />

---

<img width="1915" height="357" alt="image" src="https://github.com/user-attachments/assets/a03a2280-8517-4afb-a2f7-bd6e571309a9" />


---

<img width="1858" height="971" alt="image" src="https://github.com/user-attachments/assets/f9768eb7-d1a9-4b63-b831-147ab1d5cffa" />

---

## ⚡ Comandos rápidos

```bash
# 1) Activar entorno virtual
.\.venv\Scripts\Activate.ps1

# 2) Instalar dependencias
pip install -r requirements.txt

# 3) Correr aplicación web
python app.py

# 4) Correr suite de QA (en otra terminal)
cd qa-tests
python test_prompt_injection.py
```

---

## 🛠️ Troubleshooting

<details>
<summary><strong>❌ Error: Falta GOOGLE_API_KEY</strong></summary>

**Síntoma:** El servidor arranca pero falla al conectar con Gemini.

**Solución:**
- Verifica que exista `.env` en la raíz del proyecto
- Verifica el nombre exacto: `GOOGLE_API_KEY`
- Reinicia la terminal si acabas de crear o modificar el archivo

</details>

<details>
<summary><strong>❌ Error de conexión en QA (requests a /chat)</strong></summary>

**Síntoma:** `test_prompt_injection.py` no puede conectar al endpoint.

**Solución:**
- Confirma que `app.py` esté corriendo en `http://127.0.0.1:5000`
- Verifica que no haya otro proceso ocupando el puerto 5000

```bash
# Verificar puerto en uso (PowerShell)
netstat -ano | findstr :5000
```

</details>

<details>
<summary><strong>❌ No se genera el archivo Excel</strong></summary>

**Síntoma:** El reporte JSON y HTML se generan pero falta el `.xlsx`.

**Solución:**
```bash
pip install openpyxl
```

</details>

---

## ✅ Buenas prácticas

- 🔒 **Nunca** publiques API keys ni secretos en commits
- 📝 Mantén `payloads.json` actualizado con nuevos vectores de ataque
- 🔄 Ejecuta QA después de cualquier cambio en el prompt del sistema o lógica del backend
- 📦 Versiona los reportes importantes como evidencia de mejora continua de seguridad
