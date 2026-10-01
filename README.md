# Agentic LangChain: Secure AI Engineering & Fundamentals

A hands-on, structured guide to mastering **LangChain (v1)** and **Agentic AI systems** designed with a **security-first perspective**.

Modern LLM-powered applications and autonomous agents introduce novel attack surfaces—such as prompt injection, unauthorized tool execution, credential leakage, and untrusted output deserialization. This repository focuses on building practical LangChain workflows while highlighting defensive AI engineering best practices.

---

## Repository Highlights

- **Multi-Model Integrations**: Hands-on usage with Google Gemini (`langchain-google-genai`) and Groq high-speed inference (`langchain-groq`).
- **Autonomous Tool Use**: Wiring tools to agents safely using the principle of least privilege.
- **Message Context & Prompts**: Hardening prompts against direct and indirect injection attacks.
- **Structured Outputs with Pydantic**: Eliminating unvalidated text generation through deterministic schema validation.
- **Credential & State Hygiene**: Secure handling of secrets and environment isolation.

---

## Project Structure & Learning Path

```text
agenticLangChain/
│
├── updatedLangChain/
│   ├── 1-langchainIntro.ipynb      # LangChain v1 Core & First Agent Setup
│   ├── 2-modelIntegration.ipynb    # Model Providers (Gemini / Groq) & Fallbacks
│   ├── 3-tools.ipynb               # Function Calling & Safe Tool Integration
│   ├── 4-messages.ipynb            # Message Architectures & Context Isolation
│   └── 5-structuredOutput.ipynb    # Pydantic Schemas & Output Guardrails
│
├── .env.example                    # Sample environment template (No secrets checked in)
├── .gitignore                      # Security-centric gitignore preventing credential leaks
├── requirements.txt                # Core dependencies
└── README.md                       # Repository documentation
```

### Module Breakdown

| Module | Core Topics | Security Focus |
| :--- | :--- | :--- |
| **`1-langchainIntro.ipynb`** | LangChain v1 architecture, `init_chat_model`, first tool-enabled agent | **Environment isolation** & safe dependency initialization |
| **`2-modelIntegration.ipynb`** | Multi-provider setups (Google Gemini, Groq), system instructions | **Model credential management** and API rate limiting / quota protection |
| **`3-tools.ipynb`** | Schema definitions, tool bindings, automatic function calling (AFC) | **Tool execution boundaries**, least-privilege scoping, and input sanitization |
| **`4-messages.ipynb`** | `SystemMessage`, `HumanMessage`, `AIMessage`, multi-turn chats | **Context poisoning defense** and distinguishing system rules from untrusted inputs |
| **`5-structuredOutput.ipynb`** | `with_structured_output`, Pydantic models, field validation | **Preventing output hijacking**, hallucination mitigation, and safe downstream consumption |

---

## Security Best Practices Covered

### 1. Zero-Exposure Credential Hygiene
- API keys are never hardcoded or committed to version control.
- Isolated inside local `.env` files protected by `.gitignore`.
- Explicit use of `.env.example` to ensure secrets are never accidentally leaked.

### 2. Guardrails Against Indirect Prompt Injection
- Treating all human and external tool outputs as untrusted data.
- Enforcing explicit role separation using standard LangChain message hierarchies (`SystemMessage` vs `HumanMessage` vs `ToolMessage`).

### 3. Type-Safe Schema Enforcement (Pydantic)
- Free-form text responses can produce unexpected payloads or malicious script injections when fed into downstream APIs or databases.
- Enforcing Pydantic schemas via `.with_structured_output()` guarantees that LLM responses adhere strictly to defined types and constraints:

```python
from pydantic import BaseModel, Field

class Movie(BaseModel):
    title: str = Field(description="The validated title of the movie")
    year: int = Field(description="Year of release (validated integer)")
    director: str = Field(description="Director of the movie")
    ratings: float = Field(description="Rating score between 0.0 and 10.0")

model_with_structure = model.with_structured_output(Movie)
```

### 4. Controlled Agent Tool Execution
- Restricting tools to idempotent or read-only operations where possible.
- Applying strict argument typing to avoid arbitrary command execution or privilege escalation.

---

## Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/Ashiii27/agenticLangChain.git
cd agenticLangChain
```

### 2. Set Up a Virtual Environment
```bash
# Using python venv
python -m venv .venv

# Activate on Windows:
.venv\Scripts\activate

# Activate on macOS/Linux:
source .venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables
Copy `.env.example` to `.env` and insert your API keys:
```bash
cp .env.example .env
```

Edit `.env`:
```env
GEMINI_API_KEY="your-gemini-api-key"
GOOGLE_API_KEY="your-google-api-key"
GROQ_API_KEY="your-groq-api-key"
```

### 5. Launch the Notebooks
Open Jupyter or VS Code and navigate to `updatedLangChain/` to begin experimenting with the notebooks in sequential order.

---

## Tech Stack

- **Framework**: [LangChain](https://github.com/langchain-ai/langchain)
- **Model Providers**:
  - Google Gemini (`langchain-google-genai`)
  - Groq (`langchain-groq` - Llama 3.3, Llama 3.1)
- **Data Validation & Parsing**: [Pydantic v2](https://docs.pydantic.dev/)
- **Environment Management**: `python-dotenv`

---

## Roadmap

- [ ] Human-in-the-loop (HITL) approval workflows for critical tool executions.
- [ ] Memory persistence with encrypted conversation history.
- [ ] Retrieval-Augmented Generation (RAG) with access control and document-level permissions.
- [ ] Automated evaluation of agent security and jailbreak resistance.

---

## License

This repository is maintained for educational and research purposes. Feel free to use and adapt it for your own secure AI engineering explorations.
