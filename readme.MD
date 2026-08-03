# Proyecto: Chatbot de Soporte Interno con Escalamiento Inteligente


## 1. Narrativa de negocio


**Empresa ficticia:** "BancoAndes" — banco digital de tamaño mediano (800 empleados) que atiende tanto consultas internas de empleados (RRHH, IT) como, en una versión extendida, consultas de clientes sobre productos (tarjetas, préstamos, homebanking).


**Problema a resolver:** El área de Mesa de Ayuda interna recibe consultas repetitivas sobre políticas de compliance, procedimientos operativos y accesos a sistemas core bancarios. Además, por tratarse de una entidad financiera, hay temas que nunca pueden responderse automáticamente sin supervisión (fraude, reclamos de clientes, temas regulatorios) — lo cual hace que tu sistema de escalamiento tenga un caso de uso todavía más justificado y realista que en una empresa genérica.


**Objetivo del bot:** Responder con precisión basándose SOLO en documentación interna (RAG), y derivar a un humano cuando no tiene confianza suficiente.




## 2. Arquitectura general
```text
┌─────────────────┐
│  Streamlit UI   │  ← Chat interface + panel admin (tickets escalados)
└────────┬────────┘
        │ HTTP
┌────────▼─────────┐
│   FastAPI        │  ← Orquesta todo
│  - /chat         │
│  - /tickets      │
│  - /feedback     │
└──┬─────────┬─────┘
  │         │
  │         └──────────────┐
┌──▼──────────────┐   ┌─────▼───────────┐
│ RAG Pipeline    │   │  SQL Database   │
│ - Embeddings    │   │  - tickets      │
│ - Vector Store  │   │  - conversations│
│ - Ollama (LLM)  │   │  - feedback     │
└──┬──────────────┘   │  - documents    │
  │                  └─────────────────┘
┌──▼─────────────────┐
│ Documentos internos│
│ (RRHH, IT, FAQs)   │
└────────────────────┘
```

