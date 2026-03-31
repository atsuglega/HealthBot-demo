# 🚀 Quick Start Guide - HealthBot

Step-by-step instructions to get started in 5 minutes.

---

## ⚡ 5 Steps to Get Started

### 1️⃣ Configure Environment Variables (1 min)

Create a `.env` file in the project folder with your API keys:

```bash
# Windows/Mac/Linux - Terminal
cd c:\Users\pablo\Desktop\HealthBot-demo

# Create .env file:
echo OPENAI_API_KEY=sk-[YOUR_KEY_HERE] > .env
echo TAVILY_API_KEY=tvly-[YOUR_KEY_HERE] >> .env
```

**Where to get the keys?**
- OpenAI: https://platform.openai.com/api-keys
- Tavily: https://tavily.com

---

### 2️⃣ Open the Notebook (1 min)

**Option A: VS Code**
```bash
code healthbot.ipynb
```

**Option B: Google Colab**
- Go to https://colab.research.google.com
- Upload the `healthbot.ipynb` file
- In Colab, enter the keys when prompted

---

### 3️⃣ Install Dependencies (2 min)

**Option A: Automatic (recommended)**
- Open the notebook
- Run the first cell
- Wait for installations to complete

**Option B: Manual**
```bash
pip install -r requirements.txt
```

---

### 4️⃣ Run All Cells (1 min)

In VS Code or Colab:
- Press `Ctrl+A` (select all)
- Press `Shift+Enter` (execute)
- **Or** run each cell manually from top to bottom

You should see a green message: `✓ LangGraph graph compiled successfully`

---

### 5️⃣ Start the System (30 sec)

In the last cell, execute:

```python
run_healthbot()
```

Follow the on-screen prompts!

---

## 📚 Usage Example

```
============================================================
🎓 WELCOME TO HEALTHBOT
============================================================

📚 What topic would you like to learn about?
➜ Artificial Intelligence

[System working...]

============================================================
📖 TOPIC SUMMARY
============================================================

Artificial Intelligence (AI) is...

============================================================
❓ QUIZ QUESTION
============================================================

Which branch of AI focuses on machine learning?
A) Computer Vision
B) Machine Learning
C) Natural Language Processing
D) Robotics

✏️ Your answer (A, B, C, or D): B

[System evaluating...]

============================================================
📊 RESULTS
============================================================

Your answer: B
GRADE: 🌟 EXCELLENT

[Justification with summary citations]

============================================================
WHAT WOULD YOU LIKE TO DO?
============================================================

1. Learn a new topic
2. Exit the system

➜ Select an option: 2

👋 Thank you for using HealthBot!
```

---

## 🐛 Quick Troubleshooting

### ❌ "Error: OPENAI_API_KEY not found"
```bash
# Verify the .env file exists:
ls .env     # Windows: dir .env

# If it doesn't exist, create it:
echo OPENAI_API_KEY=sk-... > .env
echo TAVILY_API_KEY=tvly-... >> .env
```

### ❌ "ModuleNotFoundError: No module named 'langgraph'"
```bash
# Reinstall dependencies:
pip install --upgrade -r requirements.txt
```

### ❌ "No search results found"
- Try with a more general topic
- Verify your internet connection
- Check your TAVILY_API_KEY

### ❌ "Connection timeout to API"
- Increase the timeout in the configuration
- Verify that your keys are valid
- Check your API limit (quota)

---

## 📁 File Structure

```
HealthBot-demo/
├── healthbot.ipynb          ← 🎯 MAIN FILE
├── README.md                ← Complete documentation
├── ARCHITECTURE.md          ← Graph design
├── PROMPTS.md               ← Constraint explanation
├── TESTING.md               ← Testing guide
├── CONFIG.md                ← Customization
├── ROADMAP.md               ← Future improvements
├── .env.example             ← Variables template
├── .env                     ← YOUR KEYS (create)
└── requirements.txt         ← Dependencies
```

---

## 🎯 What the system does

✅ **Searches** information with Tavily (UNIQUE source)  
✅ **Generates** summary in English (3-4 paragraphs)  
✅ **Creates** quiz-type question  
✅ **Gets** your answer  
✅ **Grades** with note A-F + justification  
✅ **Allows** learning another topic or exit  

---

## 💡 Tips

- **Specific topics work better**: "Photosynthesis in aquatic plants" > "Biology"
- **Restart if errors occur**: API errors are temporary
- **Review the summary**: Understand well before answering
- **Read the justification**: Learn from your mistakes

---

## 📞 Need help?

1. Documentation: `README.md`
2. System graph: `ARCHITECTURE.md`
3. Testing guide: `TESTING.md`
4. Advanced configuration: `CONFIG.md`
5. Roadmap: `ROADMAP.md`

---

## ✨ Ready to get started!

```bash
# Summary of commands
cd c:\Users\pablo\Desktop\HealthBot-demo
code healthbot.ipynb    # Open in VS Code
# Or upload to Google Colab

# Once in the notebook:
# 1. Run all cells
# 2. In the last one, type: run_healthbot()
# 3. Start learning!
```

---

**Ready? Let's learn! 🚀**

---

Last updated: March 2026
