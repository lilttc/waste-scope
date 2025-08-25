# waste-scope
Detect what’s on a plate, estimate leftovers, and turn it into chef-friendly guidance with a tiny MLOps spine.

*"Because somewhere, grandma is still scolding you for wasting food."*

---

## 🌍 Vision
WasteScope is a mini prototype: detect food, estimate leftovers, and turn it into practical, CO₂-aware advice.  
The mission: help kitchens make better choices and reduce food waste with a gentle nudge from grandma.

---

## 🛠️ Features (planned)
- **Food Classifier** → Fine-tuned EfficientNet (PyTorch) on ~10 common food classes.  
- **Waste Estimator** → Estimate % leftovers via segmentation (heuristic + OpenMMLab tiny baseline).  
- **Advice Generator (RAG)** → Retrieve tips + CO₂ impact from a small knowledge base.  
- **Ops Spine** → FastAPI + Terraform + AWS Lambda + Athena logging.  
- **Demo App** → Streamlit upload → outputs + advice.  

---

## 📂 Repo Layout (planned)

```
waste_scope/
├─ README.md
├─ requirements.txt
├─ data/
│ ├─ sample_images/
│ └─ kb.csv
├─ notebooks/
│ ├─ 01_train_classifier.ipynb
│ └─ 02_waste_estimator.ipynb
├─ src/
│ ├─ models/
│ │ └─ classifier.py
│ ├─ rag/
│ │ └─ retrieve.py
│ ├─ io.py
│ └─ pipeline.py
├─ api/
│ └─ main.py
├─ app/
│ └─ streamlit_app.py
└─ infra/
├─ main.tf
├─ variables.tf
└─ outputs.tf

```

---

## 🚀 Roadmap
1. ✅ Repo skeleton + Food101 subset → EfficientNet baseline  
2. Waste estimator (heuristic → OpenMMLab small model)  
3. Knowledge base + RAG tips (CSV + FAISS)  
4. API + Streamlit demo  
5. Terraform infra + logging to S3/Athena  
6. Polish (README GIF, metrics, limitations, costs)  
---
