# HealthBot Flow Diagram

## LangGraph Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         SHARED STATE                                    │
│  (HealthBotState - shared between all nodes)                           │
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
                    │   1. INPUT_TOPIC (Node)           │
                    │   - Request topic from user       │
                    │   - Validate input                │
                    │   - Update state["topic"]         │
                    └──────────────────────────────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────────┐
                    │   2. SEARCH_TAVILY (Node)         │
                    │   - Execute Tavily search         │
                    │   - Get max 5 results             │
                    │   - Update state["search_...] │
                    └──────────────────────────────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────────┐
                    │   3. GENERATE_SUMMARY (Node)      │
                    │   - Based ONLY on Tavily          │
                    │   - 3-4 paragraphs in English     │
                    │   - Constraint: No knowledge      │
                    │   - Update state["summary"]       │
                    └──────────────────────────────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────────┐
                    │   4. GENERATE_QUESTION (Node)     │
                    │   - Multiple-choice question      │
                    │   - Based on summary              │
                    │   - 4 options (A,B,C,D)           │
                    │   - Update state["quiz_...]       │
                    └──────────────────────────────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────────┐
                    │   5. GET_USER_ANSWER (Node)       │
                    │   - Show summary                  │
                    │   - Show question                 │
                    │   - Request response (A/B/C/D)    │
                    │   - Validate input                │
                    └──────────────────────────────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────────┐
                    │   6. GRADE_ANSWER (Node)          │
                    │   - Evaluate user response        │
                    │   - Use ONLY summary as base      │
                    │   - Assign grade (A,B,C,D,F)      │
                    │   - Provide summary citations     │
                    └──────────────────────────────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────────┐
                    │   7. SHOW_RESULTS (Node)          │
                    │   - Show grade                    │
                    │   - Show justification            │
                    │   - Show textual citations        │
                    └──────────────────────────────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────────┐
                    │   8. DECIDE_NEXT (Node)           │
                    │   - Menu: Continue or Exit        │
                    │   - Update continue flag          │
                    └──────────────────────────────────┘
                                    │
                        ┌───────────┴───────────┐
                        │                       │
                  Continue (1)            Exit (2)
                        │                       │
                        ▼                       ▼
                  ┌──────────┐          ┌──────────┐
                  │ Restart  │          │ Terminate│
                  │  Flow    │          │  (END)   │
                  │(go to 1) │          └──────────┘
                  └────┬─────┘
                       │
                     (Loop)
```

## Data Flow Between Nodes

```
input_topic
    │ topic
    ▼
search_tavily
    │ search_results
    ▼
generate_summary (CRITICAL: ONLY Tavily)
    │ summary
    ▼
generate_question (Based on summary)
    │ quiz_question
    ▼
get_user_answer
    │ user_answer
    ▼
grade_answer (CRITICAL: ONLY summary as source)
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

## Graph Edges

### Direct Edges (Deterministic)
- `input_topic` → `search_tavily`
- `search_tavily` → `generate_summary`
- `generate_summary` → `generate_question`
- `generate_question` → `get_user_answer`
- `get_user_answer` → `grade_answer`
- `grade_answer` → `show_results`
- `show_results` → `decide_next`

### Conditional Edges (Dynamic)
```python
def should_continue(state):
    if state["continue_learning"]:
        return "input_topic"  # Restart
    else:
        return "end"  # Terminate
```

- `decide_next` → `input_topic` (if continue_learning == True)
- `decide_next` → `__end__` (if continue_learning == False)

## Data Constraints

### Node: generate_summary
```
📥 Allowed Input:
   - state["search_results"] ✅
   
📛 NOT Allowed Input:
   - Prior LLM knowledge ❌
   - Internet information (except Tavily) ❌
   
📤 Output:
   - state["summary"]: String with 3-4 paragraphs
```

### Node: generate_question
```
📥 Allowed Input:
   - state["summary"] ✅
   
📛 NOT Allowed Input:
   - state["search_results"] ❌
   - Prior LLM knowledge ❌
   
📤 Output:
   - state["quiz_question"]: String with question + options
```

### Node: grade_answer
```
📥 Allowed Input:
   - state["summary"] ✅
   - state["user_answer"] ✅
   - state["quiz_question"] ✅
   
📛 NOT Allowed Input:
   - state["search_results"] ❌
   - Prior LLM knowledge ❌
   
📤 Output:
   - state["grade"]: String (A/B/C/D/F)
   - state["justification"]: String with citations
```

## State Lifecycle

```
Iteration 1:
├─ topic: "Artificial Intelligence"
├─ search_results: [result1, result2, ...]
├─ summary: "AI is..."
├─ quiz_question: "What is...?"
├─ user_answer: "A"
├─ grade: "A"
├─ continue_learning: true
│
Iteration 2 (restart):
├─ topic: "Machine Learning"  ← New input
├─ search_results: [...]         ← New results
├─ summary: "ML is..."           ← New summary
├─ quiz_question: "How...?"      ← New question
├─ user_answer: "C"              ← New response
├─ grade: "B"                     ← New grade
├─ continue_learning: false       ← Final decision
│
End of flow
```

## Implemented Validations

```python
# Validation 1: User input
validate_input = topic.strip() != ""

# Validation 2: Successful search
has_results = len(search_results) > 0

# Validation 3: Generated summary (not empty)
summary_valid = len(summary) > 100

# Validation 4: Valid response
answer_valid = user_answer in ["A", "B", "C", "D"]

# Validation 5: Continuity option
continue_choice_valid = choice in ["1", "2"]
```

## Error Handling

```
try:
    ├─ Tavily API Error
    │  └─ → Show message, use empty state
    │
    ├─ OpenAI API Error
    │  └─ → Show message, use default response
    │
    ├─ Input Validation Error
    │  └─ → Request re-entry
    │
    └─ General Exception
       └─ → Log, continue or abort gracefully
```
