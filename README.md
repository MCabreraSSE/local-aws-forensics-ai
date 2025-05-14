# 🕵️ Local AWS Cloud Forensics AI

**Automatización de análisis forense en AWS usando Ollama y modelos LLM locales**. Clasifica eventos de CloudTrail, detecta amenazas según MITRE ATT&CK Cloud, y genera dashboards interactivos **sin enviar datos a la nube**.

![Dashboard Preview](src/dashboard/assets/demo.gif)

## 🚀 Características
- 🔍 **Clasificación automática** de eventos AWS (S3, IAM, EC2) con Ollama + Llama 3.
- 📊 **Búsqueda semántica** en logs usando ChromaDB y embeddings locales.
- 🚨 **Alertas en tiempo real** via Telegram/Slack.
- 💻 100% ejecutable en **entornos locales** (Docker o bare-metal).

## 🛠️ Tecnologías
| **Área**         | **Herramientas**                                                                 |
|------------------|---------------------------------------------------------------------------------|
| LLM              | Ollama + Llama 3 (70B) / Mistral (7B)                                           |
| Procesamiento    | LangChain, Pandas, PyTorch (SentenceTransformers)                               |
| Almacenamiento   | MinIO (S3 local), ChromaDB, PostgreSQL                                          |
| Visualización    | Streamlit / Grafana                                                             |
| Infraestructura  | Docker, Kubernetes (k3s)                                                        |


## Configurar entorno
cp .env.example .env  # Editar con tus credenciales AWS
docker-compose up -d  # Inicia Ollama + MinIO

 ## Descargar modelos LLM
ollama pull llama3:70b
ollama pull mistral

## Procesar logs de ejemplo
python src/data_processing/aws_connector.py --days 7  # Descarga logs de CloudTrail
python src/llm/classifier.py  # Clasifica eventos

## Lanzar dashboard
streamlit run src/dashboard/streamlit_app.py


/local-aws-forensics-ai
│
├── /data                # Datos de ejemplo y logs
│   ├── raw/             # Logs crudos de CloudTrail (JSON)
│   └── processed/       # Datos normalizados (OCSF format)
│
├── /src
│   ├── data_processing/ 
│   │   ├── aws_connector.py   # Descarga logs via AWS CLI
│   │   └── normalizer.py      # Normalización a OCSF
│   │
│   ├── llm/              
│   │   ├── classifier.py      # Clasificación con Ollama
│   │   ├── embeddings.py      # Generación de vectores
│   │   └── prompts/           # Templates de prompts
│   │
│   ├── storage/         
│   │   ├── chroma_db.py       # Vector database setup
│   │   └── minio_utils.py     # Alternativa local a S3
│   │
│   ├── api/             
│   │   ├── fastapi_app.py     # API REST para consultas
│   │   └── alerts.py          # Notificaciones (Telegram/Slack)
│   │
│   └── dashboard/       
│       ├── streamlit_app.py   # Visualización interactiva
│       └── assets/            # Imágenes/estilos
│
├── /notebooks           # Experimentos y análisis
│   ├── EDA.ipynb        # Análisis exploratorio
│   └── Testing.ipynb    # Validación de modelos
│
├── /infra               # Despliegue
│   ├── docker-compose.yml  # Ollama + API + MinIO
│   └── Dockerfile       # Entorno Python
│
├── .env.example         # Variables de entorno
├── requirements.txt     # Dependencias Python
└── README.md           # Documentación principal
