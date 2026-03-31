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

✏️ Your answer (A, B, C or D): A

⏳ Evaluating your response...

============================================================
📊 YOUR EVALUATION RESULTS
============================================================

Your answer: A

GRADE: 🌟 EXCELLENT

JUSTIFICATION:
Your answer is correct. The summary indicates that...
SUMMARY CITATION: "Machine Learning is the branch of AI..."

============================================================
WHAT WOULD YOU LIKE TO DO?
============================================================

1. Learn a new topic
2. Exit the system

➜ Select an option (1 or 2): 2

============================================================
👋 Thank you for using HealthBot!
============================================================

Keep learning! 📚
```

---

## 🎯 Expected Results

When running the system, you should observe:

### ✅ Web Search (Tavily)
```
🔍 Searching for information about: [Your topic]
✓ Found 5 results from real sources
  1. https://source1.com
  2. https://source2.com
  ...
```

### ✅ Content Generation
```
📝 Summary generated (3-4 coherent paragraphs)
✓ Based exclusively on search results
✓ In English, accessible language
```

### ✅ Intelligent Evaluation
```
Grade: A (Excellent)
Justification: [Detailed explanation]
Material citation: "[Relevant fragment]"
```

### ✅ Interactive Experience
- Natural conversational flow
- Menu to continue learning or exit
- Infinite loop of education

---

## 🧪 Testing and Validation

### Option 1: Demo Mode (Without API Keys)
Ideal for quick demonstration without spending money:
```bash
python test_healthbot.py --demo
```
Runs with **simulated data** (result guaranteed in <1 second)

### Option 2: Full Testing (With APIs)
Validates real integration with OpenAI and Tavily:
```bash
python test_healthbot.py
```
Requires `OPENAI_API_KEY` and `TAVILY_API_KEY` in `.env`

### Option 3: Interactive Use
Complete educational experience:
```bash
jupyter notebook healthbot.ipynb
# Run all cells
# Run: run_healthbot()
```

---

## 📋 Requirements Checklist

### Functional ✅
- [x] Request learning topic
- [x] Use Tavily as search tool
- [x] Generate summary (3-4 paragraphs)
- [x] Create quiz-type question
- [x] Request user response
- [x] Grade (A-F) with justification and citations
- [x] Allow restart/exit

### Technical ✅
- [x] Implement with LangGraph
- [x] Shared state updated by nodes
- [x] Each node with single responsibility
- [x] Conditional edges for flow
- [x] Tavily as external tool
- [x] OpenAI for generation and evaluation

### Constraints ✅
- [x] NO prior knowledge
- [x] NO external sources (only Tavily)
- [x] Summary exclusively from Tavily
- [x] Question from summary
- [x] Evaluation based on summary

---

## 🐛 Troubleshooting

### "Error: OPENAI_API_KEY not found"
→ Make sure you have a `.env` file with your OpenAI key

### "Error: TAVILY_API_KEY not found"
→ Sign up at [tavily.com](https://tavily.com) and add your key to `.env`

### "No search results found"
→ Try with a more general topic or check your internet connection

### "Error: Graph cannot be compiled"
→ Verify all node functions are defined before `create_healthbot_graph()`

---

## 📝 License

This project was developed as part of a learning exercise with LangGraph, OpenAI, and Tavily.

---

## 👨‍💻 Author

Developed as an educational demonstration system.

---

## 📚 Referencias

- [LangGraph Documentación](https://github.com/langchain-ai/langgraph)
- [OpenAI API](https://platform.openai.com)
- [Tavily Search](https://tavily.com)
- [LangChain](https://python.langchain.com)

---

**Última actualización**: Marzo 2026
**Versión**: 1.0
