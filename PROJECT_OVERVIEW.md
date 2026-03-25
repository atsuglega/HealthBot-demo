# 📋 PROJECT OVERVIEW - HealthBot

## Resumen Ejecutivo

**HealthBot** es un sistema de educación interactivo desarrollado como proyecto final que integra **Inteligencia Artificial Generativa**, **Orquestación de Workflows** y **Web Scraping** para crear un entorno de aprendizaje personalizado y validado.

---

## 🎓 Objetivos del Proyecto

### Objetivo Principal
Desarrollar un **sistema educativo inteligente** que:
- Proporcione material de aprendizaje de alta calidad basado en fuentes reales (web)
- Genere evaluaciones personalizadas sobre cualquier tema
- Proporcione retroalimentación inmediata y fundamentada

### Objetivos Secundarios
1. Demostrar integración de **LLMs modernos** (OpenAI GPT-4o-mini)
2. Implementar **orquestación compleja de workflows** con LangGraph
3. Garantizar **integridad de datos**: solo usar fuentes externas (Tavily)
4. Crear **interfaz conversacional fluida** para educación

---

## 🛠️ Tecnologías Utilizadas

### Stack Principal
| Componente | Tecnología | Propósito |
|-----------|-----------|----------|
| **LLM Backend** | OpenAI GPT-4o-mini | Generación de contenido educativo |
| **Orquestación** | LangGraph (StateGraph) | Gestión del flujo de datos y lógica |
| **Búsqueda Web** | Tavily Search API | Obtención de información verificable |
| **Integración** | LangChain Framework | Abstracción y composición de componentes |
| **Entorno** | Python 3.13 + Jupyter | Desarrollo e interacción |

### Justificación Técnica
- **GPT-4o-mini**: Potencia suficiente para educación, costo-optimizado
- **LangGraph**: Garantiza secuencia correcta de operaciones y gestión de estado
- **Tavily**: Búsqueda en tiempo real, evita alucinaciones basadas en conocimiento estático
- **LangChain**: Estándar industrial para aplicaciones con LLMs

---

## 🏗️ Arquitectura Implementada

### Patrón: Agentic Workflow con Estado Persistente

```
[Input] → [Search] → [Summarize] → [Question] → [Answer] → [Grade] → [Output]
                                                               ↓
                                                        [Menu → Repeat?]
```

### Componentes Clave

#### 1. **State Management (TypedDict)**
```python
HealthBotState = {
    topic: str                    # Tema de aprendizaje
    search_results: list[dict]    # Datos de Tavily (única fuente)
    summary: str                  # Resumen educativo
    quiz_question: str            # Pregunta de evaluación
    user_answer: str              # Respuesta (A/B/C/D)
    grade: str                    # Calificación (A-F)
    justification: str            # Explicación + evidencia
}
```

#### 2. **8 Nodos del Workflow**
1. **input_topic**: Captura intención del usuario
2. **search_tavily**: Obtiene información verificable (web real-time)
3. **generate_summary**: Síntesis educativa (3-4 párrafos)
4. **generate_question**: Quiz de opción múltiple
5. **get_user_answer**: Interfaz usuario
6. **grade_answer**: Evaluación inteligente + justificación
7. **show_results**: Presentación de feedback
8. **decide_next**: Control de flujo (continuar/salir)

#### 3. **Aristas Condicionales**
- Loop de aprendizaje continuo: `decide_next` → `input_topic` si continúa
- Terminación elegante: `decide_next` → END si el usuario sale

---

## ✨ Características Diferenciales

### 1. **Integridad de Datos Verificados**
✅ Información **ÚNICAMENTE de Tavily** (web real-time)  
✅ No usa conocimiento pre-entrenado del modelo  
✅ Citas textuales en respuestas  

### 2. **Educación Fundamentada**
✅ Resúmenes de 3-4 párrafos coherentes  
✅ Preguntas respondibles desde el material  
✅ Calificación justificada (A-F)  

### 3. **Experiencia Interactiva**
✅ Conversación natural en español  
✅ Loop de aprendizaje continuo  
✅ Feedback inmediato  

### 4. **Arquitectura Escalable**
✅ Fácil agregar nuevos tipos de evaluaciones  
✅ Modular: cambiar LLM sin afectar flujo  
✅ Extensible: integrar nuevas fuentes de datos  

---

## 📊 Resultados Esperados

### Demostración de Competencias
- ✅ **AI Integration**: Uso profesional de APIs (OpenAI, Tavily)
- ✅ **Software Architecture**: State management, workflow orchestration
- ✅ **Problem Solving**: Restricciones implementadas y verificadas
- ✅ **Best Practices**: Manejo seguro de credenciales, documentación clara

### Testing & Validación
- Sistema de testing automatizado (`test_healthbot.py`)
- Documentación detallada de arquitectura
- Ejemplo de ejecución con datos reales

---

## 🚀 Cómo Usar Este Proyecto

### Opción 1: Ejecución Interactiva (Completa)
```bash
# 1. Configurar .env con tus API keys
# 2. Ejecutar el notebook: healthbot.ipynb
# 3. Correr: run_healthbot()
```

### Opción 2: Testing Automatizado
```bash
# Sin requerir interacción manual
python test_healthbot.py
```

### Opción 3: Demo (Sin API Calls)
```bash
# Ver el flujo sin costar dinero en API keys
python test_healthbot.py --demo
```

---

## 📁 Estructura del Proyecto

```
HealthBot-demo/
├── healthbot.ipynb              ⭐ Sistema completo (interactivo)
├── test_healthbot.py            🧪 Suite de testing automatizado
├── requirements.txt             📦 Dependencias Python
├── .env.example                 🔑 Plantilla de configuración
├── CONFIG.md                    ⚙️  Instrucciones de setup
├── ARCHITECTURE.md              🏗️  Detalles técnicos profundos
├── QUICKSTART.md                🚀 Guía rápida
└── README.md                    📖 Documentación general
```

---

## 🔒 Consideraciones de Seguridad

### API Keys
- ✅ `.env` **NO incluido** en repositorio (`.gitignore`)
- ✅ Usar `.env.example` como plantilla
- ✅ Nunca commitear credenciales reales

### Cost Control
- GPT-4o-mini: $0.00015/1K tokens (económico)
- ~$0.001-0.005 por consulta educativa
- Alertas de límite recomendables en OpenAI

---

## 📚 Referencias y Dependencias

| Librería | Versión | Uso |
|----------|---------|-----|
| `langchain-openai` | Latest | ChatOpenAI |
| `langgraph` | Latest | StateGraph, workflow |
| `langchain-core` | Latest | Mensajes, integraciones |
| `langchain-community` | Latest | Tavily Search |
| `python-dotenv` | Latest | Variables de entorno |

---

## 👨‍💻 Conclusiones Técnicas

Este proyecto demuestra:

1. **Competencia en AI/ML**: Integración profesional de LLMs y APIs
2. **Arquitectura de Software**: Diseño modular y extensible
3. **Resolución de Problemas**: Implementación de restricciones complejas
4. **Documentación**: Claridad y profesionalismo
5. **Best Practices**: Seguridad, testing, mantenibilidad

El sistema está **listo para producción** con las debidas configuraciones de escala y monitoreo.

---

**Fecha de entrega**: Marzo 2026  
**Estado**: ✅ Completo y validado  
**Repositorio**: https://github.com/atsuglega/HealthBot-demo
