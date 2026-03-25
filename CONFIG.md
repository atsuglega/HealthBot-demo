# Configuración de HealthBot

Este archivo permite personalizar el comportamiento del sistema sin modificar el código del notebook.

---

## 🎛️ Parámetros Configurables

### OpenAI Configuration

```python
# Modelo a usar
OPENAI_MODEL = "gpt-4o-mini"  # Opciones: gpt-4, gpt-4-turbo, gpt-3.5-turbo
# Nota: gpt-4o-mini es recomendado por rendimiento/costo

# Temperatura (creatividad del modelo)
LLM_TEMPERATURE = 0.7  # Rango: 0.0 (determinístico) - 1.0 (creativo)
# Recomendaciones:
#   - 0.3-0.5 para tareas precisas
#   - 0.7 para resúmenes naturales
#   - 0.9+ para generación creativa

# Max tokens por respuesta
LLM_MAX_TOKENS = 1000  # Aumentar para resúmenes más largos
```

### Tavily Configuration

```python
# Máximo número de resultados de búsqueda
TAVILY_MAX_RESULTS = 5  # Rango: 1-10
# Notas:
#   - 5 es balance entre calidad y variedad
#   - +10 puede agregar ruido
#   - -3 puede dejar gaps de información

# Incluir respuesta generada por Tavily
TAVILY_INCLUDE_ANSWER = False  # True si quieres la respuesta resume de Tavily
```

### Sistema Configuration

```python
# Idioma de salida
SYSTEM_LANGUAGE = "es"  # "es" (español), "en" (inglés), etc.

# Modo verbose (más logs)
VERBOSE_MODE = True  # True para debug, False para producción

# Mostrar emojis en salida
USE_EMOJIS = True  # True, False

# Timeout para API calls (segundos)
API_TIMEOUT = 30  # Aumentar para conexiones lentas
```

### Quiz Configuration

```python
# Número de párrafos del resumen
SUMMARY_PARAGRAPHS = 3  # Rango: 2-5
# Nota: Los requisitos especifican 3-4

# Número de opciones de múltiple opción
QUIZ_OPTIONS = 4  # Típicamente: 4 (A,B,C,D)

# Escala de calificación
GRADE_SCALE = {
    "A": (90, 100),  # 90-100%
    "B": (80, 89),   # 80-89%
    "C": (70, 79),   # 70-79%
    "D": (60, 69),   # 60-69%
    "F": (0, 59)     # 0-59%
}
```

### Flujo Configuration

```python
# Permitir reiniciar ciclo
ALLOW_RESTART = True  # True para permitir nuevo tema

# Máximo de ciclos por sesión
MAX_CYCLES = 999  # Cambiar a número si quieres límite

# Mostrar tiempo de ejecución
SHOW_EXECUTION_TIME = True
```

---

## 🔧 Modificación de Parámetros

### Opción 1: Variables de Entorno (.env)

```bash
# En el archivo .env:
OPENAI_MODEL=gpt-4-turbo
LLM_TEMPERATURE=0.5
TAVILY_MAX_RESULTS=3
VERBOSE_MODE=True
```

### Opción 2: Código (en el notebook)

Antes de ejecutar `run_healthbot()`, redefine:

```python
# Personalizar configuración
OPENAI_MODEL = "gpt-4-turbo"
LLM_TEMPERATURE = 0.5
TAVILY_MAX_RESULTS = 3

# Reinicializar herramientas
llm = ChatOpenAI(model=OPENAI_MODEL, temperature=LLM_TEMPERATURE)
search_tool = TavilySearchResults(max_results=TAVILY_MAX_RESULTS)

# Luego ejecutar
run_healthbot()
```

### Opción 3: Crear archivo de configuración

```python
# config.py
CONFIG = {
    "openai": {
        "model": "gpt-4o-mini",
        "temperature": 0.7,
        "max_tokens": 1000
    },
    "tavily": {
        "max_results": 5
    },
    "system": {
        "language": "es",
        "verbose": True,
        "emojis": True
    }
}

# En el notebook:
# from config import CONFIG
```

---

## 📊 Perfiles Predefinidos

### Perfil: "Educativo" (Default)
```python
# Equilibrio entre precisión y variedad
OPENAI_MODEL = "gpt-4o-mini"
LLM_TEMPERATURE = 0.7
TAVILY_MAX_RESULTS = 5
SUMMARY_PARAGRAPHS = 3
```

### Perfil: "Preciso"
```python
# Máxima precisión, respuestas determinísticas
OPENAI_MODEL = "gpt-4o-mini"
LLM_TEMPERATURE = 0.3
TAVILY_MAX_RESULTS = 3
SUMMARY_PARAGRAPHS = 3
```

### Perfil: "Creativo"
```python
# Resúmenes más elaborados
OPENAI_MODEL = "gpt-4-turbo"
LLM_TEMPERATURE = 0.9
TAVILY_MAX_RESULTS = 10
SUMMARY_PARAGRAPHS = 4
```

### Perfil: "Rápido" (Bajo costo)
```python
# Optimizado para velocidad y costo
OPENAI_MODEL = "gpt-3.5-turbo"
LLM_TEMPERATURE = 0.7
TAVILY_MAX_RESULTS = 3
```

---

## 🌍 Configuración Multi-idioma

El sistema puede adaptarse a otros idiomas modificando los prompts:

### Español (Default)
```python
SYSTEM_LANGUAGE = "es"
```

### Inglés
```python
SYSTEM_LANGUAGE = "en"
# Modificar prompts en los nodos para idioma inglés
```

### Francés
```python
SYSTEM_LANGUAGE = "fr"
```

---

## 🚀 Optimizaciones de Rendimiento

### Para reducir tiempo de ejecución:
```python
TAVILY_MAX_RESULTS = 3  # Menos búsquedas, más rápido
LLM_TEMPERATURE = 0.3   # Respuestas más cortas
OPENAI_MODEL = "gpt-3.5-turbo"  # Más rápido
```

### Para mejor calidad:
```python
TAVILY_MAX_RESULTS = 10  # Más información
LLM_TEMPERATURE = 0.7    # Más variedad
OPENAI_MODEL = "gpt-4"   # Mayor capacidad
```

### Para menor costo:
```python
OPENAI_MODEL = "gpt-3.5-turbo"  # Mucho más barato
TAVILY_MAX_RESULTS = 3
API_TIMEOUT = 20  # Fallar rápido si hay error
```

---

## 🔐 Configuración de Seguridad

```python
# Rate limiting (evitar demasiadas llamadas)
RATE_LIMIT_PER_MINUTE = 10
RATE_LIMIT_PER_HOUR = 100

# Validación de entrada
MAX_INPUT_LENGTH = 500  # Caracteres máximos para el tema
BLOCKED_KEYWORDS = ["hack", "exploit"]  # Palabras bloqueadas

# Logging
LOG_TO_FILE = True
LOG_FILENAME = "healthbot.log"
```

---

## 📋 Validación de Configuración

Para verificar que tu configuración es válida:

```python
def validate_config():
    """Valida los parámetros de configuración"""
    assert 0 <= LLM_TEMPERATURE <= 1, "Temperatura fuera de rango"
    assert 1 <= TAVILY_MAX_RESULTS <= 10, "Máximo de resultados inválido"
    assert 2 <= SUMMARY_PARAGRAPHS <= 5, "Párrafos de resumen inválido"
    assert OPENAI_MODEL in ["gpt-4", "gpt-4-turbo", "gpt-3.5-turbo", "gpt-4o-mini"]
    print("✅ Configuración válida")

# validate_config()
```

---

## 🧪 Experimentación Recomendada

### Experimento 1: Efecto de Temperatura
```
Mantener todo igual, variar LLM_TEMPERATURE
- 0.3: Respuestas conservadoras
- 0.7: Respuestas balanceadas (DEFAULT)
- 0.9: Respuestas creativas
```

### Experimento 2: Efecto de Número de Resultados
```
Mantener todo igual, variar TAVILY_MAX_RESULTS
- 3: Rápido, superficial
- 5: Balanceado (DEFAULT)
- 10: Completo, más lento
```

### Experimento 3: Efecto de Modelo
```
Mantener todo igual, variar OPENAI_MODEL
- gpt-3.5-turbo: Barato, rápido
- gpt-4o-mini: Equilibrado (DEFAULT)
- gpt-4: Preciso, caro
```

---

## 📈 Métricas de Performance

Para rastrear rendimiento:

```python
import time

class PerformanceMetrics:
    def __init__(self):
        self.start_time = None
        self.end_time = None
        self.search_time = 0
        self.summary_time = 0
        self.question_time = 0
        self.grade_time = 0
    
    @property
    def total_time(self):
        return self.end_time - self.start_time
    
    def print_report(self):
        print(f"⏱️ Búsqueda: {self.search_time:.2f}s")
        print(f"⏱️ Resumen: {self.summary_time:.2f}s")
        print(f"⏱️ Pregunta: {self.question_time:.2f}s")
        print(f"⏱️ Calificación: {self.grade_time:.2f}s")
        print(f"⏱️ TOTAL: {self.total_time:.2f}s")

# metrics = PerformanceMetrics()
```

---

## 🎯 Configuración Recomendada por Caso de Uso

### Caso: Educación en Tiempo Real (Classroom)
```python
OPENAI_MODEL = "gpt-3.5-turbo"  # Rápido
LLM_TEMPERATURE = 0.5  # Consistente
TAVILY_MAX_RESULTS = 5
USE_EMOJIS = True  # Más amigable
```

### Caso: Investigación Académica
```python
OPENAI_MODEL = "gpt-4-turbo"  # Preciso
LLM_TEMPERATURE = 0.3  # Determinístico
TAVILY_MAX_RESULTS = 10  # Exhaustivo
SUMMARY_PARAGRAPHS = 4  # Detallado
```

### Caso: Auto-aprendizaje Personal
```python
OPENAI_MODEL = "gpt-4o-mini"  # Equilibrado
LLM_TEMPERATURE = 0.7  # Interesante
TAVILY_MAX_RESULTS = 5
MAX_CYCLES = 10  # Límite amigable
```

---

**Última actualización**: Marzo 2026  
**Versión de configuración**: 1.0
