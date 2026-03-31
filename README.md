# HealthBot - Interactive AI-Powered Educational System

## 📚 Overview

**HealthBot** is an intelligent educational system that integrates **Generative Language Models** (OpenAI GPT-4o-mini), **Complex Workflow Orchestration** (LangGraph), and **Verified Web Information Retrieval** (Tavily Search) to provide an interactive, personalized, and data-driven learning experience.

### 🎯 Purpose

Create an educational platform that enables users to:
- Learn about any topic with verifiable web content
- Be evaluated with automatically generated questions
- Receive immediate and justifiable feedback
- Continue learning in an interactive loop

### 🔄 Workflow Flow

1. **Intent Capture**: User enters a learning topic
2. **Verified Search**: Real-time information retrieval from web (Tavily Search)
3. **Educational Synthesis**: Coherent summary generation (3-4 paragraphs in English)
4. **Formative Evaluation**: Multiple-choice question creation based on content
5. **Response Interface**: Interactive user response capture (A/B/C/D)
6. **Intelligent Analysis**: Automated evaluation with detailed justification (A-F)
7. **Continuous Iteration**: Loop allowing continued learning or exit

---

## 🔧 Technical Requirements

### Main Dependencies

```
langgraph          # Workflow orchestration
langchain          # LLM and tool integration
langchain-openai   # OpenAI GPT-4 model
langchain-tavily   # Tavily Search integration
python-dotenv      # Environment variable management
```

### Required API Keys

- **OpenAI**: [https://platform.openai.com/api-keys](https://platform.openai.com/api-keys)
  - Model: `gpt-4o-mini`
  
- **Tavily**: [https://tavily.com](https://tavily.com)
  - Web search with up to 5 results per query

---

## ⚙️ Configuration and Installation

### 1. Clone or download the project

```bash
cd HealthBot-demo
```

### 2. Create environment variables

Create a `.env` file in the project root:

```env
OPENAI_API_KEY=sk-your-key-here
TAVILY_API_KEY=tvly-your-key-here
```

**Alternative**: If using Google Colab, you can let the notebook request the keys at runtime.

### 3. Run the notebook

**Option A - VS Code**:
```bash
jupyter notebook healthbot.ipynb
```

**Option B - Google Colab**:
- Upload the `healthbot.ipynb` file to Google Colab
- Dependencies will be installed automatically

---

## 🚀 Execution

### Step 1: Run all cells

Press `Ctrl+A` then `Shift+Enter` to execute all cells in order, or run each one manually from top to bottom.

### Step 2: Start the system

In the last code cell, execute:

```python
run_healthbot()
```

### Step 3: Interact with the system

The system will guide you through each step with interactive prompts.

---

## 📊 Project Architecture

### Main Components

#### 1. **Shared State (HealthBotState)**
```python
{
    "topic": str                # Topic entered
    "search_results": dict      # Tavily results
    "summary": str              # Generated summary
    "quiz_question": str        # Quiz question
    "user_answer": str          # User response (A,B,C,D)
    "grade": str                # Grade (A-F)
    "justification": str        # Explanation + citations
    "continue_learning": bool   # Continue?
    "current_step": str         # Current step
}
```

#### 2. **Graph Nodes**

| Node | Function | Input | Output |
|------|----------|-------|--------|
| `input_topic` | Requests user topic | Interactive I/O | topic |
| `search_tavily` | Searches information | topic | search_results |
| `generate_summary` | Creates summary (3-4 paragraphs) | search_results | summary |
| `generate_question` | Creates quiz question | summary | quiz_question |
| `get_user_answer` | Gets user response | quiz_question | user_answer |
| `grade_answer` | Grades response | summary + user_answer | grade + justification |
| `show_results` | Shows evaluation | grade + justification | (display) |
| `decide_next` | Continue/exit menu | (interactive) | continue_learning |

#### 3. **Conditional Edges**

- **After `decide_next`**:
  - `continue_learning == True` → Returns to `input_topic`
  - `continue_learning == False` → Ends (END)

---

## 🎯 Implemented Constraints

### ✅ Validated

1. **Do NOT use model previous knowledge**
   - Explicit prompts: "Based ONLY on..."
   
2. **Do NOT use external sources other than Tavily**
   - Only `search_results` passed to content generation
   
3. **Summary based 100% on Tavily**
   - Context built exclusively from `search_results`
   
4. **Question answerable from summary**
   - Generated from summary content
   
5. **Grading with textual citations**
   - Justification includes summary excerpts

---

## 🧪 Execution Example

```
============================================================
🎓 WELCOME TO HEALTHBOT - Interactive Learning System
============================================================

📚 What topic would you like to learn about?
➜ Artificial Intelligence

✓ Topic selected: Artificial Intelligence

🔍 Searching for information about: Artificial Intelligence
✓ Found 5 information sources
  1. wikipedia.org
  2. arxiv.org
  3. github.com
  ...

📝 Generating summary about: Artificial Intelligence
✓ Summary generated successfully

❓ Generating question about the topic...
✓ Question generated successfully

============================================================
📖 TOPIC SUMMARY
============================================================

Artificial Intelligence (AI) is...

============================================================
❓ QUIZ QUESTION
============================================================

Which branch of AI focuses on...?
A) Machine Learning
B) Deep Learning
C) Natural Language Processing
D) Computer Vision

✏️ Tu respuesta (A, B, C o D): A

⏳ Evaluando tu respuesta...

============================================================
📊 RESULTADOS DE TU EVALUACIÓN
============================================================

Tu respuesta: A

CALIFICACIÓN: 🌟 EXCELENTE

JUSTIFICACIÓN:
Tu respuesta es correcta. El resumen indica que...
CITA DEL RESUMEN: "Machine Learning es la rama de la IA..."

============================================================
¿QUÉ DESEAS HACER?
============================================================

1. Aprender un nuevo tema
2. Salir del sistema

➜ Selecciona una opción (1 o 2): 2

============================================================
👋 ¡Gracias por usar HealthBot!
============================================================

¡Que sigas aprendiendo! 📚
```

---

## 🎯 Resultados Esperados

Al ejecutar el sistema, deberías observar:

### ✅ Búsqueda Web (Tavily)
```
🔍 Buscando información sobre: [Tu tema]
✓ Se encontraron 5 resultados desde fuentes reales
  1. https://source1.com
  2. https://source2.com
  ...
```

### ✅ Generación de Contenido
```
📝 Resumen generado (3-4 párrafos coherentes)
✓ Basado exclusivamente en resultados de búsqueda
✓ En español, lenguaje accesible
```

### ✅ Evaluación Inteligente
```
Calificación: A (Excelente)
Justificación: [Explicación detallada]
Cita del material: "[Fragmento relevante]"
```

### ✅ Experiencia Interactiva
- Flujo conversacional natural
- Menú para continuar aprendiendo o salir
- Loop infinito de educación

---

## 🧪 Testing y Validación

### Opción 1: Modo Demo (Sin API Keys)
Ideal para demostración rápida sin costar dinero:
```bash
python test_healthbot.py --demo
```
Ejecuta con **datos simulados** (resultado garantizado en <1 segundo)

### Opción 2: Testing Completo (Con APIs)
Valida integración real con OpenAI y Tavily:
```bash
python test_healthbot.py
```
Requiere `OPENAI_API_KEY` y `TAVILY_API_KEY` en `.env`

### Opción 3: Uso Interactivo
Experiencia educativa completa:
```bash
jupyter notebook healthbot.ipynb
# Ejecutar todas las celdas
# Correr: run_healthbot()
```

---

## 📋 Checklist de Requisitos

### Funcionales ✅
- [x] Solicitar tema de aprendizaje
- [x] Usar Tavily como herramienta de búsqueda
- [x] Generar resumen en español (3-4 párrafos)
- [x] Crear pregunta tipo quiz
- [x] Solicitar respuesta del usuario
- [x] Calificar (A-F) con justificación y citas
- [x] Permitir reinicio/salida

### Técnicos ✅
- [x] Implementar con LangGraph
- [x] Estado compartido actualizado por nodos
- [x] Cada nodo con responsabilidad única
- [x] Aristas condicionales para flujo
- [x] Tavily como herramienta externa
- [x] OpenAI para generación y evaluación

### Restricciones ✅
- [x] NO conocimiento previo
- [x] NO fuentes externas (solo Tavily)
- [x] Resumen exclusivamente de Tavily
- [x] Pregunta desde resumen
- [x] Evaluación basada en resumen

---

## 🐛 Solución de Problemas

### "Error: OPENAI_API_KEY no configurada"
→ Asegúrate de tener un archivo `.env` con tu clave de OpenAI

### "Error: TAVILY_API_KEY no configurada"
→ Regístrate en [tavily.com](https://tavily.com) y añade tu clave al `.env`

### "No se encontraron resultados de búsqueda"
→ Intenta con un tema más general o verifica tu conexión a internet

### "Error: El grafo no puede ser compilado"
→ Verifica que todas las funciones de nodos estén definidas antes de `create_healthbot_graph()`

---

## 📝 Licencia

Este proyecto fue desarrollado como parte de un ejercicio de aprendizaje con LangGraph, OpenAI y Tavily.

---

## 👨‍💻 Autor

Desarrollado como sistema de demostración educativa.

---

## 📚 Referencias

- [LangGraph Documentación](https://github.com/langchain-ai/langgraph)
- [OpenAI API](https://platform.openai.com)
- [Tavily Search](https://tavily.com)
- [LangChain](https://python.langchain.com)

---

**Última actualización**: Marzo 2026
**Versión**: 1.0
