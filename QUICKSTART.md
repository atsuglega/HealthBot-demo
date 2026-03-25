# 🚀 Guía Rápida de Inicio - HealthBot

Instrucciones paso a paso para empezar en 5 minutos.

---

## ⚡ 5 Pasos para Comenzar

### 1️⃣ Configurar Variables de Entorno (1 min)

Crea un archivo `.env` en la carpeta del proyecto con tus API keys:

```bash
# Windows/Mac/Linux - Terminal
cd c:\Users\pablo\Desktop\HealthBot-demo

# Crea archivo .env:
echo OPENAI_API_KEY=sk-[YOUR_KEY_HERE] > .env
echo TAVILY_API_KEY=tvly-[YOUR_KEY_HERE] >> .env
```

**¿Dónde obtener las keys?**
- OpenAI: https://platform.openai.com/api-keys
- Tavily: https://tavily.com

---

### 2️⃣ Abrir el Notebook (1 min)

**Opción A: VS Code**
```bash
code healthbot.ipynb
```

**Opción B: Google Colab**
- Ve a https://colab.research.google.com
- Sube el archivo `healthbot.ipynb`
- En Colab, escribe las keys cuando se solicite

---

### 3️⃣ Instalar Dependencias (2 min)

**Opción A: Automático (recomendado)**
- Abre el notebook
- Ejecuta la primera celda
- Esperapara que terminen las instalaciones

**Opción B: Manual**
```bash
pip install -r requirements.txt
```

---

### 4️⃣ Ejecutar Todas las Celdas (1 min)

En VS Code o Colab:
- Presiona `Ctrl+A` (seleccionar todo)
- Presiona `Shift+Enter` (ejecutar)
- **O** ejecuta cada celda de arriba a abajo manualmente

Verás un mensaje verde: `✓ Grafo de LangGraph compilado exitosamente`

---

### 5️⃣ Iniciar el Sistema (30 seg)

En la última celda, ejecuta:

```python
run_healthbot()
```

¡Sigue los prompts en pantalla!

---

## 📚 Ejemplo de Uso

```
============================================================
🎓 BIENVENIDO A HEALTHBOT
============================================================

📚 ¿Cuál es el tema que deseas aprender?
➜ Inteligencia Artificial

[Sistema trabaja...]

============================================================
📖 RESUMEN DEL TEMA
============================================================

La Inteligencia Artificial (IA) es...

============================================================
❓ PREGUNTA DEL QUIZ
============================================================

¿Cuál es la rama de la IA que se enfoca en machine learning?
A) Computer Vision
B) Machine Learning
C) Natural Language Processing
D) Robotics

✏️ Tu respuesta (A, B, C o D): B

[Sistema evalúa...]

============================================================
📊 RESULTADOS
============================================================

Tu respuesta: B
CALIFICACIÓN: 🌟 EXCELENTE

[Justificación con citas del resumen]

============================================================
¿QUÉ DESEAS HACER?
============================================================

1. Aprender un nuevo tema
2. Salir del sistema

➜ Selecciona una opción: 2

👋 ¡Gracias por usar HealthBot!
```

---

## 🐛 Solución Rápida de Problemas

### ❌ "Error: OPENAI_API_KEY not found"
```bash
# Verifica que el archivo .env existe:
ls .env     # Windows: dir .env

# Si no existe, créalo:
echo OPENAI_API_KEY=sk-... > .env
echo TAVILY_API_KEY=tvly-... >> .env
```

### ❌ "ModuleNotFoundError: No module named 'langgraph'"
```bash
# Reinstala dependencias:
pip install --upgrade -r requirements.txt
```

### ❌ "No se encontraron resultados de búsqueda"
- Intenta con un tema más general
- Verifica tu conexión a internet
- Verifica tu TAVILY_API_KEY

### ❌ "Connection timeout al API"
- Aumenta el timeout en la configuración
- Verifica que tus keys sean válidas
- Revisa tu límite de API (quota)

---

## 📁 Estructura de Archivos

```
HealthBot-demo/
├── healthbot.ipynb          ← 🎯 ARCHIVO PRINCIPAL
├── README.md                ← Documentación completa
├── ARCHITECTURE.md          ← Diseño del grafo
├── PROMPTS.md               ← Explicación de restricciones
├── TESTING.md               ← Guía de pruebas
├── CONFIG.md                ← Personalización
├── ROADMAP.md               ← Mejoras futuras
├── .env.example             ← Plantilla de variables
├── .env                     ← TUS KEYS (crear)
└── requirements.txt         ← Dependencias
```

---

## 🎯 Qué hace el sistema

✅ **Busca** información con Tavily (ÚNICA fuente)  
✅ **Genera** resumen en español (3-4 párrafos)  
✅ **Crea** pregunta tipo quiz  
✅ **Obtiene** tu respuesta  
✅ **Califica** con nota A-F + justificación  
✅ **Permite** aprender otro tema o salir  

---

## 💡 Tips

- **Temas específicos funcionan mejor**: "Fotosíntesis en plantas acuáticas" > "Biología"
- **Reinicia si hay errores**: Los errores de API son temporales
- **Revisa el resumen**: Entiende bien antes de responder
- **Lee la justificación**: Aprende de los errores

---

## 📞 Necesitas ayuda?

1. Documentación: `README.md`
2. Grafo del sistema: `ARCHITECTURE.md`
3. Guía de tests: `TESTING.md`
4. Configuración avanzada: `CONFIG.md`
5. Roadmap: `ROADMAP.md`

---

## ✨ ¡Listo para empezar!

```bash
# Resumen de comandos
cd c:\Users\pablo\Desktop\HealthBot-demo
code healthbot.ipynb    # Abrir en VS Code
# O subir a Google Colab

# Una vez en el notebook:
# 1. Ejecuta todas las celdas
# 2. En la última, escribe: run_healthbot()
# 3. ¡Comienza a aprender!
```

---

**¿Estás listo? ¡Vamos a aprender! 🚀**

---

Última actualización: Marzo 2026
