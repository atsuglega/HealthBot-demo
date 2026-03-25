# HealthBot - Sistema de Aprendizaje Interactivo con LangGraph

## 📚 Descripción del Proyecto

HealthBot es un sistema educativo interactivo que utiliza **LangGraph**, **OpenAI** y **Tavily** para crear una experiencia de aprendizaje personalizada y validada.

### Flujo de funcionamiento

1. **Solicitud de tema**: El usuario ingresa un tema de aprendizaje
2. **Búsqueda con Tavily**: Sistema busca información relevante (única fuente)
3. **Generación de resumen**: Crea 3-4 párrafos en español basados **EXCLUSIVAMENTE** en Tavily
4. **Quiz**: Genera una pregunta de opción múltiple desde el resumen
5. **Respuesta del usuario**: Usuario selecciona su respuesta (A, B, C, D)
6. **Calificación inteligente**: 
   - Nota: A (Excelente) → F (Insuficiente)
   - Justificación basada en el resumen
   - Citas textuales del contenido
7. **Menú de continuidad**: Permite aprender otro tema o salir

---

## 🔧 Requisitos Técnicos

### Dependencias principales

```
langgraph          # Orquestación de flujos
langchain          # Integración de LLMs y herramientas
langchain-openai   # Modelo GPT-4 de OpenAI
langchain-tavily   # Integración con Tavily Search
python-dotenv      # Gestión de variables de entorno
```

### API Keys necesarias

- **OpenAI**: [https://platform.openai.com/api-keys](https://platform.openai.com/api-keys)
  - Modelo: `gpt-4o-mini`
  
- **Tavily**: [https://tavily.com](https://tavily.com)
  - Búsqueda web con hasta 5 resultados por consulta

---

## ⚙️ Configuración e Instalación

### 1. Clonar o descargar el proyecto

```bash
cd HealthBot-demo
```

### 2. Crear variables de entorno

Crea un archivo `.env` en la carpeta raíz:

```env
OPENAI_API_KEY=sk-your-key-here
TAVILY_API_KEY=tvly-your-key-here
```

**Alternativa**: Si usas Google Colab, puedes dejar que el notebook solicite las keys en tiempo de ejecución.

### 3. Ejecutar el notebook

**Opción A - VS Code**:
```bash
jupyter notebook healthbot.ipynb
```

**Opción B - Google Colab**:
- Sube el archivo `healthbot.ipynb` a Google Colab
- Las dependencias se instalarán automáticamente

---

## 🚀 Ejecución

### Paso 1: Ejecutar todas las celdas

Presiona `Ctrl+A` y luego `Shift+Enter` para ejecutar todas las celdas en orden, o ejecuta cada una manualmente desde arriba hacia abajo.

### Paso 2: Iniciar el sistema

En la última celda de código, ejecuta:

```python
run_healthbot()
```

### Paso 3: Interactuar con el sistema

El sistema te guiará a través de cada paso con prompts interactivos.

---

## 📊 Arquitectura del Proyecto

### Componentes principales

#### 1. **Estado Compartido (HealthBotState)**
```python
{
    "topic": str                # Tema ingresado
    "search_results": dict      # Resultados de Tavily
    "summary": str              # Resumen generado
    "quiz_question": str        # Pregunta del quiz
    "user_answer": str          # Respuesta (A,B,C,D)
    "grade": str                # Calificación (A-F)
    "justification": str        # Explicación + citas
    "continue_learning": bool   # ¿Continuar?
    "current_step": str         # Paso actual
}
```

#### 2. **Nodos del Grafo**

| Nodo | Función | Entrada | Salida |
|------|---------|---------|--------|
| `input_topic` | Solicita tema del usuario | InteractivoI/O | topic |
| `search_tavily` | Busca información | topic | search_results |
| `generate_summary` | Crea resumen (3-4 párrafos) | search_results | summary |
| `generate_question` | Crea pregunta tipo quiz | summary | quiz_question |
| `get_user_answer` | Obtiene respuesta del usuario | quiz_question | user_answer |
| `grade_answer` | Califica respuesta | summary + user_answer | grade + justification |
| `show_results` | Muestra evaluación | grade + justification | (display) |
| `decide_next` | Menú continuar/salir | (interactive) | continue_learning |

#### 3. **Aristas Condicionales**

- **Después de `decide_next`**:
  - `continue_learning == True` → Regresa a `input_topic`
  - `continue_learning == False` → Termina (END)

---

## 🎯 Restricciones Implementadas

### ✅ Validadas

1. **NO usar conocimiento previo del modelo**
   - Prompts explícitos: "Basándote ÚNICAMENTE en..."
   
2. **NO usar fuentes externas distintas de Tavily**
   - Solo `search_results` se pasan a generación de contenido
   
3. **Resumen basado 100% en Tavily**
   - Contexto construido exclusivamente desde `search_results`
   
4. **Pregunta respondible desde resumen**
   - Generada del contenido del resumen
   
5. **Calificación con citas textuales**
   - Justificación incluye fragmentos del resumen

---

## 🧪 Ejemplo de Ejecución

```
============================================================
🎓 BIENVENIDO A HEALTHBOT - Sistema de Aprendizaje Interactivo
============================================================

📚 ¿Cuál es el tema que deseas aprender?
➜ Inteligencia Artificial

✓ Tema seleccionado: Inteligencia Artificial

🔍 Buscando información sobre: Inteligencia Artificial
✓ Se encontraron 5 fuentes de información
  1. wikipedia.org
  2. arxiv.org
  3. github.com
  ...

📝 Generando resumen sobre: Inteligencia Artificial
✓ Resumen generado exitosamente

❓ Generando pregunta sobre el tema...
✓ Pregunta generada exitosamente

============================================================
📖 RESUMEN DEL TEMA
============================================================

La Inteligencia Artificial (IA) es...

============================================================
❓ PREGUNTA DEL QUIZ
============================================================

¿Cuál es la rama de la IA que se enfoca en...?
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
