# EASL Summer School — Hands-on LLMs

Practical session introducing large language models, retrieval-augmented generation (RAG), and agentic AI in the context of EASL clinical guidelines.

---

## Before you start

### API key
Each group receives a shared API key at the start of the session. It connects to a hosted model endpoint — you do not need an OpenAI account.

Store it in a `.env` file in the same folder as the notebooks:
```
KATHER_API_KEY=sk-your-group-key-here
```
Never paste it directly into a notebook cell.

### Environment — local recommended over Colab
Running the notebooks locally gives you faster iteration, persistent state between cells, and proper `.env` support. The notebooks work in both environments, but local is preferred.

**Local setup:**
```bash
git clone https://github.com/JanClusmann/easl_school_material
cd easl_school_material
python -m venv .venv
# Windows:  .venv\Scripts\activate
# Mac/Linux: source .venv/bin/activate
pip install -r requirements.txt
```
Then open the `.ipynb` files in VS Code or JupyterLab.

**Google Colab fallback:**
Upload the notebook and the relevant PDF/Excel files manually. The notebooks detect Colab automatically and switch to `getpass` / `files.upload()` where needed.

---

## Structure

```
Notebook 1 — Exploration          (everyone starts here)
│
├── Case 1: MASLD          ← mandatory for all groups
│   ├── Easy    — zero-shot LLM
│   ├── Medium  — RAG with EASL 2024 MASLD guideline
│   └── Advanced — agentic AI
│
└── Cases 2 & 3, Section 4, Section 5  ← choose your branch below
```

---

## Step 1 — Everyone: Case 1 (Notebook 1)

Open **`Notebook 1 Exploration EASL_Summer_School.ipynb`** and work through **Case 1 — MASLD** from top to bottom:

| Task | What you build |
|------|---------------|
| Easy | Ask a zero-shot LLM the working diagnosis; explore temperature, system prompts, and other API parameters |
| Medium | Implement a RAG pipeline over the EASL 2024 MASLD guideline PDF and query it |
| Advanced | Build an agent that autonomously retrieves and compares two versions of the guideline |

Complete Case 1 before splitting into groups. The RAG pipeline you build here (`chunks`, `index`, `answer_question`) is reused in all branches.

---

## Step 2 — Choose your branch

After Case 1, groups split. At the end of the session **each group presents their work interactively** — use whatever format you like (live demo, slides, notebook walkthrough, web app).

---

### Branch A — Cases 2 & 3 (Notebook 1, continued)

Continue in **Notebook 1** with the remaining clinical cases:

- **Case 2 — Chronic Hepatitis B:** antiviral therapy decisions, comparing 2017 vs. 2025 EASL HBV guidelines with RAG, rule-based treatment recommendation agent
- **Case 3 — Autoimmune Hepatitis:** first-line treatment, guideline comparison, agent that browses the web for contraindications and queries the CAMARO trial

**Goal:** present what you learned about prompt engineering, RAG, and agentic AI across all three disease areas. Compare how the models handle guideline knowledge with and without retrieval.

---

### Branch B — Evaluation Suite (Notebook 2)

Open **`Notebook 2 Systematic Eval.ipynb`**.

A benchmark of 10 MASLD-related clinical questions with clinician-provided ground truth answers is available in `benchmark_data.xlsx`.

| Step | What you do |
|------|------------|
| Setup | Load environment, connect to API, build RAG pipeline |
| Config | Select which models to evaluate; set `TEST_MODE = True` for a quick sanity check |
| Run | Collect zero-shot and RAG answers from each model for all 10 questions |
| Results | Inspect answers alongside retrieved guideline excerpts; save to `benchmark_results.xlsx` |
| Present | Visualise zero-shot vs. RAG performance quantitatively |

**Optional extension:** Build a web app (Section 5 of Notebook 1, using Gradio) where clinicians can read LLM answers and rate them — an interactive annotation interface for the benchmark.

**Goal:** show a rigorous, quantitative comparison of model performance with and without retrieval, and discuss what the numbers reveal about the limitations of zero-shot LLMs for guideline-based reasoning.

---

### Branch C — Guideline Chat Web App (Notebook 1, Section 5)

Build a clinical decision-support chat application — similar in spirit to OpenEvidence — that lets clinicians ask free-text questions and receive guideline-grounded answers.

Start from the **Gradio app template in Section 5** of Notebook 1 and extend it:

- Upload the MASLD (and optionally HBV or AIH) guideline PDF
- Build a RAG backend that retrieves the most relevant guideline sections per query
- Present the answer alongside the source excerpts so clinicians can verify the reasoning
- Use the questions in `benchmark_data.xlsx` as a test set to demo the app

**Goal:** live demo of the finished app — show a clinician asking a question and getting a guideline-backed answer with citations. Discuss what it would take to make this production-ready.

---

## Files in this repository

| File | Description |
|------|-------------|
| `Notebook 1 Exploration EASL_Summer_School.ipynb` | Main exploration notebook — Cases 1–3, evaluation intro, Gradio app |
| `Notebook 2 Systematic Eval.ipynb` | Standalone evaluation notebook — benchmark runner, multi-model comparison |
| `benchmark_data.xlsx` | 10 MASLD benchmark questions with clinician ground truth |
| `EASL 2024 MASLD MetALD.pdf` | Guideline PDF used in Case 1 RAG pipeline |
| `.env.example` | Template for your API key file |

---

## Tips

- **Keep `.env` out of git.** It is already listed in `.gitignore`. Never commit real keys.
- **`TEST_MODE = True`** in Notebook 2 runs only the first question — use it to verify your setup before the full run (which makes ~40 API calls).
- **RAG retrieval quality matters.** Print the retrieved chunks (`🔍 Retrieved guideline excerpts`) to understand what the model actually read before generating an answer.
- **LLM-as-judge** (instructor solution) will be shared after the session so you can compare your qualitative impressions with automated scores.
