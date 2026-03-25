# Diagrama de Flujo de HealthBot

## Arquitectura del Grafo de LangGraph

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         ESTADO COMPARTIDO                               │
│  (HealthBotState - compartido entre todos los nodos)                   │
└─────────────────────────────────────────────────────────────────────────┘
                                    ▲
                                    │
        ┌───────────────────────────┼───────────────────────────┐
        │                           │                           │
        │                           │                           │
    ┌───▼────┐             ┌──────▼──────┐           ┌────────▼───┐
    │ Topic  │             │   Search    │           │   Summary  │
    │ String │             │   Results   │           │   String   │
    └────────┘             │   Dict      │           └────────────┘
                           └─────────────┘
                                    │
                                    │
                                    ▼
                    ┌──────────────────────────────────┐
                    │   1. INPUT_TOPIC (Nodo)           │
                    │   - Solicitar tema del usuario    │
                    │   - Validar entrada               │
                    │   - Actualizar state["topic"]     │
                    └──────────────────────────────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────────┐
                    │   2. SEARCH_TAVILY (Nodo)         │
                    │   - Ejecutar búsqueda con Tavily  │
                    │   - Obtener 5 resultados máximo   │
                    │   - Actualizar state["search_...] │
                    └──────────────────────────────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────────┐
                    │   3. GENERATE_SUMMARY (Nodo)      │
                    │   - Basado ÚNICAMENTE en Tavily   │
                    │   - 3-4 párrafos en español       │
                    │   - Restricción: No conocimiento  │
                    │   - Actualizar state["summary"]   │
                    └──────────────────────────────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────────┐
                    │   4. GENERATE_QUESTION (Nodo)     │
                    │   - Pregunta tipo opción múltiple │
                    │   - Basada en resumen             │
                    │   - 4 opciones (A,B,C,D)          │
                    │   - Actualizar state["quiz_...]   │
                    └──────────────────────────────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────────┐
                    │   5. GET_USER_ANSWER (Nodo)       │
                    │   - Mostrar resumen               │
                    │   - Mostrar pregunta              │
                    │   - Solicitar respuesta (A/B/C/D) │
                    │   - Validar entrada               │
                    └──────────────────────────────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────────┐
                    │   6. GRADE_ANSWER (Nodo)          │
                    │   - Evaluar respuesta del usuario │
                    │   - Usar SOLO resumen como base   │
                    │   - Asignar nota (A,B,C,D,F)      │
                    │   - Proporcionar citas del resumen│
                    └──────────────────────────────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────────┐
                    │   7. SHOW_RESULTS (Nodo)          │
                    │   - Mostrar calificación          │
                    │   - Mostrar justificación         │
                    │   - Mostrar citas textuales       │
                    └──────────────────────────────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────────┐
                    │   8. DECIDE_NEXT (Nodo)           │
                    │   - Menú: Continuar o Salir       │
                    │   - Actualizar flag continue      │
                    └──────────────────────────────────┘
                                    │
                        ┌───────────┴───────────┐
                        │                       │
                  Continue (1)            Salir (2)
                        │                       │
                        ▼                       ▼
                  ┌──────────┐          ┌──────────┐
                  │ Reiniciar│          │ Terminar │
                  │  Flujo   │          │  (END)   │
                  │(ir a 1)  │          └──────────┘
                  └────┬─────┘
                       │
                     (Loop)
```

## Flujo de Datos entre Nodos

```
input_topic
    │ topic
    ▼
search_tavily
    │ search_results
    ▼
generate_summary (CRÍTICO: SOLO Tavily)
    │ summary
    ▼
generate_question (Basado en summary)
    │ quiz_question
    ▼
get_user_answer
    │ user_answer
    ▼
grade_answer (CRÍTICO: SOLO summary como fuente)
    │ grade, justification
    ▼
show_results
    │ (display)
    ▼
decide_next
    │ continue_learning
    ├─ True → input_topic (Loop)
    └─ False → END
```

## Aristas del Grafo

### Aristas directas (Determinísticas)
- `input_topic` → `search_tavily`
- `search_tavily` → `generate_summary`
- `generate_summary` → `generate_question`
- `generate_question` → `get_user_answer`
- `get_user_answer` → `grade_answer`
- `grade_answer` → `show_results`
- `show_results` → `decide_next`

### Aristas condicionales (Dinámicas)
```python
def should_continue(state):
    if state["continue_learning"]:
        return "input_topic"  # Reiniciar
    else:
        return "end"  # Terminar
```

- `decide_next` → `input_topic` (si continue_learning == True)
- `decide_next` → `__end__` (si continue_learning == False)

## Restricciones de Datos

### Nodo: generate_summary
```
📥 Entrada permitida:
   - state["search_results"] ✅
   
📛 Entrada NO permitida:
   - Knowledge previo del LLM ❌
   - Información de internet (excepto Tavily) ❌
   
📤 Salida:
   - state["summary"]: String con 3-4 párrafos
```

### Nodo: generate_question
```
📥 Entrada permitida:
   - state["summary"] ✅
   
📛 Entrada NO permitida:
   - state["search_results"] ❌
   - Knowledge previo del LLM ❌
   
📤 Salida:
   - state["quiz_question"]: String con pregunta + opciones
```

### Nodo: grade_answer
```
📥 Entrada permitida:
   - state["summary"] ✅
   - state["user_answer"] ✅
   - state["quiz_question"] ✅
   
📛 Entrada NO permitida:
   - state["search_results"] ❌
   - Knowledge previo del LLM ❌
   
📤 Salida:
   - state["grade"]: String (A/B/C/D/F)
   - state["justification"]: String con citas
```

## Ciclo de Vida del Estado

```
Iteración 1:
├─ topic: "Inteligencia Artificial"
├─ search_results: [resultado1, resultado2, ...]
├─ summary: "La IA es..."
├─ quiz_question: "¿Cuál es...?"
├─ user_answer: "A"
├─ grade: "A"
├─ continue_learning: true
│
Iteración 2 (reinicio):
├─ topic: "Machine Learning"  ← Nueva entrada
├─ search_results: [...]         ← Nuevos resultados
├─ summary: "ML es..."           ← Nuevo resumen
├─ quiz_question: "¿Cómo...?"    ← Nueva pregunta
├─ user_answer: "C"              ← Nueva respuesta
├─ grade: "B"                     ← Nueva calificación
├─ continue_learning: false       ← Decisión final
│
Fin del flujo
```

## Validaciones Implementadas

```python
# Validación 1: Entrada del usuario
validate_input = topic.strip() != ""

# Validación 2: Búsqueda exitosa
has_results = len(search_results) > 0

# Validación 3: Resumen generado (no vacío)
summary_valid = len(summary) > 100

# Validación 4: Respuesta válida
answer_valid = user_answer in ["A", "B", "C", "D"]

# Validación 5: Opción de continuidad
continue_choice_valid = choice in ["1", "2"]
```

## Manejo de Errores

```
try:
    ├─ Tavily API Error
    │  └─ → Mostrar mensaje, usar estado vacío
    │
    ├─ OpenAI API Error
    │  └─ → Mostrar mensaje, usar respuesta por defecto
    │
    ├─ Input Validation Error
    │  └─ → Solicitar reingreso
    │
    └─ General Exception
       └─ → Registrar, continuar o abortar gracefully
```
