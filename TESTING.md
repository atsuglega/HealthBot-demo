# Guía de Pruebas y Validación de HealthBot

Esta guía te ayuda a validar que el sistema funciona correctamente y que se cumplen todas las restricciones.

---

## 📋 Checklist de Validación

### ✅ Instalación y Configuración

- [ ] Archivo `.env` creado con OPENAI_API_KEY
- [ ] Archivo `.env` creado con TAVILY_API_KEY
- [ ] Dependencias instaladas: `pip install -r requirements.txt`
- [ ] Notebook abierto sin errores de importación
- [ ] Primera celda ejecutada sin errores de instalación

### ✅ Prueba 1: Búsqueda con Tavily

**Objetivo**: Verificar que Tavily retorna resultados válidos

**Pasos**:
1. Ejecuta todas las celdas hasta "search_tavily"
2. Ingresa un tema que conoces: "Python programming"
3. Verifica que se muestren al menos 3 fuentes

**Validación**:
```python
# En la celda de búsqueda, deberías ver:
# ✓ Se encontraron X fuentes de información
# 1. source1.com
# 2. source2.com
# etc.
```

**Criterio de éxito**: ✅ Si ves fuentes reales (wikipedia, github, etc.)

---

### ✅ Prueba 2: Resumen Basado ÚNICAMENTE en Tavily

**Objetivo**: Verificar que el resumen NO usa conocimiento previo

**Pasos**:
1. Elige un tema POCO COMÚN: "Historia de los viajes espaciales de Corea del Norte"
2. Deja que Tavily busque
3. Lee el resumen generado
4. Verifica que SOLO contiene información de las fuentes mostradas

**Validaciones Manuales**:
- [ ] El resumen menciona solo información de las fuentes
- [ ] El resumen no agrega contexto no mencionado en búsqueda
- [ ] El resumen está en español
- [ ] El resumen tiene 3-4 párrafos

**Prueba de Falsificación**:
Si conoces el tema muy bien, busca información que NO debería estar en el resumen.

Ejemplo correcto:
```
Tema: "Inteligencia Artificial en Madagascar"
Búsqueda: [solo 3 artículos, 1 menciona IA, 1 Madagascar, 1 ambos]
Resumen: Solo combina lo que está en los 3 artículos, sin agregar
```

**Criterio de éxito**: ✅ El resumen parece "superficial" pero correcto (solo Tavily)

---

### ✅ Prueba 3: Pregunta Basada en Resumen

**Objetivo**: Verificar que la pregunta es respondible desde el resumen

**Pasos**:
1. Lee el resumen completo
2. Lee la pregunta generada
3. Intenta responder la pregunta SOLO con información del resumen
4. Verifica que la respuesta correcta según OpenAI esté en el resumen

**Validaciones**:
- [ ] Puedo responder la pregunta leyendo SOLO el resumen
- [ ] No necesito información externa
- [ ] Las 4 opciones parecen plausibles
- [ ] Una opción es claramente correcta según el resumen

**Ejemplo incorrecto [NO debería ocurrir]**:
```
Resumen habla de: "La IA es..."
Pregunta: "¿Cuándo se inventó la IA?" 
❌ Imposible responder desde el resumen porque no lo menciona
```

**Ejemplo correcto**:
```
Resumen dice: "La fotosíntesis convierte CO2 y H2O en glucosa..."
Pregunta: "¿Qué se necesita para la fotosíntesis?"
✅ Respondible: CO2 y agua (mencionados en resumen)
```

**Criterio de éxito**: ✅ La pregunta es respondible desde el resumen

---

### ✅ Prueba 4: Validación de Respuesta

**Objetivo**: Verificar que la calificación usa SOLO el resumen

**Pasos**:
1. Lee el resumen
2. Lee la pregunta
3. Selecciona una respuesta (correcta o incorrecta)
4. Observa la justificación

**Validaciones**:
- [ ] La justificación cita fragmentos del resumen
- [ ] La explicación se basa en lo que dice el resumen
- [ ] Si contestaste mal, la justificación explica POR QUÉ es incorrecta según resumen
- [ ] NO hay información additiva de fuentes externas

**Ejemplo correcto**:
```
Tu respuesta: A
JUSTIFICACIÓN: Tu respuesta es correcta. El resumen afirma que 
"la ecuación es 6CO2 + 6H2O..." y la opción A dice exactamente eso.
CITA: "6CO2 + 6H2O + luz → C6H12O6 + 6O2"
```

**Ejemplo incorrecto [NO debería ocurrir]**:
```
Tu respuesta: C
JUSTIFICACIÓN: Tu respuesta es incorrecta. Se sabe que la fotosíntesis
requiere clorofila, mitocondrias, cilios, flagelos...
❌ Esto usa conocimiento externo, no citas del resumen
```

**Criterio de éxito**: ✅ Todas las justificaciones citan el resumen

---

### ✅ Prueba 5: Ciclo Completo

**Objetivo**: Verificar que el flujo completo funciona

**Pasos**:
1. Ejecuta `run_healthbot()`
2. Ingresa tema: "Energía solar"
3. Espera a que se busque y genere resumen
4. Lee el resumen completo
5. Responde la pregunta
6. Revisa la calificación
7. Selecciona "Continuar" (opción 1)
8. Ingresa tema: "Viajes en el tiempo"
9. Repite pasos 3-6
10. Selecciona "Salir" (opción 2)

**Validaciones**:
- [ ] Todo funciona sin errores
- [ ] Puedo continuar a un segundo tema
- [ ] El estado se reinicia correctamente
- [ ] La aplicación termina gracefully

**Criterio de éxito**: ✅ Flujo completo sin errores

---

## 🧪 Pruebas Avanzadas

### Prueba A: Tema Muy General

**Objetivo**: Verificar que el sistema funciona con temas amplios

```
Tema: "Inteligencia Artificial"
Resultado esperado: Resumen general pero correcto
```

---

### Prueba B: Tema Muy Específico

**Objetivo**: Verificar que el sistema maneja temas niche

```
Tema: "Algoritmo de árbol de decisiones en biodiversidad marina del Sudeste Asiático"
Resultado esperado: Resumen específico de lo encontrado en Tavily
```

---

### Prueba C: Tema Científico

**Objetivo**: Verificar que el sistema maneja ecuaciones y datos

```
Tema: "Ecuación de Schrödinger en mecánica cuántica"
Validación: ¿El resumen incluye la ecuación si Tavily la proporciona?
```

---

### Prueba D: Tema Histórico

**Objetivo**: Verificar que el sistema maneja cronologías

```
Tema: "Historia de la Imprenta"
Validación: ¿Las fechas son consistentes con Tavily?
Validación: ¿La pregunta relacionada con fechas es respondible?
```

---

### Prueba E: Respuesta Incorrecta Intencionada

**Objetivo**: Verificar que la calificación reduce nota cuando es incorrecta

```
Tema: "Agua"
Pregunta generada: "¿Cuál es la fórmula del agua?"
Tu respuesta intencionalmente: La opción INCORRECTA
Resultado esperado: 
  - CALIFICACIÓN: D o F
  - JUSTIFICACIÓN: Explicación de por qué es incorrecta
```

---

### Prueba F: Tema en Otro Idioma (Español)

**Objetivo**: Verificar que el sistema funciona con temas en español

```
Tema: "La Revolución Francesa"
Validación: ¿El resumen está en español?
Validación: ¿La pregunta está en español?
```

---

## 📊 Registro de Pruebas

### Plantilla para documentar resultados

```markdown
## Sesión de Pruebas: [Fecha]

### Prueba 1: Búsqueda Tavily
- Tema: ___________
- Fuentes encontradas: [ ] Sí [ ] No
- Cantidad: ___
- Fuentes validas: [ ] Sí [ ] No

### Prueba 2: Resumen
- Párrafos: ___
- Solo Tavily: [ ] Sí [ ] No
- Español: [ ] Sí [ ] No
- Detalles adicionales: ___________

### Prueba 3: Pregunta
- Respondible desde resumen: [ ] Sí [ ] No
- 4 opciones: [ ] Sí [ ] No
- Claridad: [ ] Alta [ ] Media [ ] Baja

### Prueba 4: Calificación
- Citas incluidas: [ ] Sí [ ] No
- Justificación clara: [ ] Sí [ ] No
- Solo resumen: [ ] Sí [ ] No

### Prueba 5: Ciclo Completo
- Sin errores: [ ] Sí [ ] No
- Reinicio funciona: [ ] Sí [ ] No
- Salida correcta: [ ] Sí [ ] No

### Notas Generales:
_______________________________________________
```

---

## 🐛 Debugging

### Problema: "Error: OPENAI_API_KEY no configurada"

**Solución**:
```bash
# Opción 1: Crear archivo .env
echo "OPENAI_API_KEY=sk-..." > .env

# Opción 2: En Google Colab
from google.colab import userdata
openai_key = userdata.get('OPENAI_API_KEY')
```

### Problema: "No se encontraron resultados de búsqueda"

**Solución**:
- Intenta con un tema más general
- Verifica tu conexión a internet
- Verifica tu TAVILY_API_KEY

### Problema: "La pregunta no se puede responder desde el resumen"

**Información**:
- Esto PUEDE ocurrir si el LLM no genera bien
- No es un error crítico del sistema
- Intenta con un tema diferente

### Problema: Calificación siempre es "C"

**Posible causa**:
- Podría haber error en parsing de la respuesta de OpenAI
- Verifica que el output incluya "CALIFICACIÓN: A/B/C/D/F"

### Problema: Ciclo infinito

**Solución**:
- Presiona `Ctrl+C` para interrumpir
- El sistema debería mostrar "⚠️ Sesión interrumpida"

---

## ✅ Matriz de Validación Final

| Componente | Prueba | Estado | Notas |
|-----------|--------|--------|-------|
| Importaciones | Celdas se ejecutan | ✓/✗ | |
| Tavily | Obtiene resultados | ✓/✗ | |
| Summary | Solo Tavily | ✓/✗ | |
| Question | Del resumen | ✓/✗ | |
| Grading | Con citas | ✓/✗ | |
| Cycling | Reinicio | ✓/✗ | |
| Exit | Salida limpia | ✓/✗ | |

---

## 🎉 Criterio de Aceptación

El sistema se considera **FUNCIONAL** cuando:

- ✅ Todos los componentes funcionan sin errores
- ✅ El resumen es SOLO de Tavily (auditado manualmente)
- ✅ La pregunta es respondible desde resumen
- ✅ La calificación cita el resumen
- ✅ El ciclo completo se puede ejecutar
- ✅ Se puede reiniciar y salir sin errores

---

## 📝 Reportar Problemas

Si encuentras un problema, documenta:

1. **Tema ingresado**
2. **Error recibido** (mensaje completo)
3. **Pasos para reproducir**
4. **Resultado esperado vs. actual**
5. **API Keys verificadas** (sin mostrar valores reales)

Ejemplo:
```
Tema: "Fotosíntesis"
Error: OpenAI returned 401 Unauthorized
Pasos: Ejecuté todas las celdas en orden
Esperado: Resumen generado
Actual: Error en generate_summary
Validación: Las keys están correctas (primeros chars OK)
```

---

**Última actualización**: Marzo 2026  
**Versión de pruebas**: 1.0
