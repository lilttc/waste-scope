# WasteScope 🍽️
Detect what’s on a plate, estimate leftovers, and turn it into **chef-friendly, chat-based guidance** with a tiny MLOps spine.

*"Because somewhere, grandma is still scolding you for wasting food."*

---

## 🌍 Vision
WasteScope is a mini-Orbisk prototype: detect food, estimate leftovers, and turn it into practical, **CO₂-aware advice** that chefs can **chat** with. The goal: help kitchens make better choices and reduce food waste, with a gentle nudge from grandma.

---

## 🛠️ Features (planned)
- **Food Classifier** → Fine-tuned EfficientNet (PyTorch) on ~10 common food classes.  
- **Waste Estimator** → % leftovers via heuristic segmentation + (stretch) tiny OpenMMLab baseline.  
- **Agentic ChefBot** → LangChain agent with tools: `waste_lookup`, `co2_advice`, `forecast_savings`.  
- **Ops Spine** → FastAPI endpoints (`/predict`, `/chat`) + Terraform (S3, Lambda, API GW, Glue, Athena) + CI.  
- **Demo App** → Streamlit upload (analyze) **and** chat (ask follow-ups, what-ifs).

---

## 🧩 Architecture (words not pictures)
**Streamlit (upload + chat)** → **FastAPI** (`/predict`, `/chat`) →  
- **PyTorch classifier** (EfficientNet)  
- **Waste estimator** (heuristic → MMSeg stretch)  
- **Agent (LangChain)** using FAISS + `kb.csv` and tools (`waste_lookup`, `co2_advice`, `forecast_savings`)  
→ JSON `{class, waste_pct, co2e, tips[3]}` + chat answers → **log to S3 (Parquet)** → Glue → **Athena**.

---

## 📂 Repo Layout (planned)


```

waste-scope/
├─ README.md
├─ requirements.txt
├─ data/
│ ├─ sample_images/
│ └─ kb.csv # item, co2e_per_100g, tips, source
├─ notebooks/
│ ├─ 01_train_classifier.ipynb
│ └─ 02_waste_estimator.ipynb
├─ src/
│ ├─ models/
│ │ ├─ classifier.py # PyTorch load/predict
│ │ └─ waste.py # heuristic or MMSeg inference
│ ├─ rag/
│ │ ├─ build_index.py # embed kb.csv (sentence-transformers)
│ │ └─ retrieve.py
│ ├─ tools/ # agent-callable tools
│ │ ├─ waste_lookup.py # recent predictions (Athena/local)
│ │ ├─ co2_kb.py # CO₂ lookup + tips
│ │ └─ forecast_savings.py # grams/€ / CO₂ calculator (toy)
│ ├─ agent/
│ │ └─ chef_bot.py # LangChain agent wrapper
│ ├─ io.py # s3 utils; Athena writer
│ ├─ schemas.py # Pydantic DTOs
│ └─ pipeline.py # predict → estimate → advise
├─ api/
│ └─ main.py # FastAPI /docs ( /predict , /chat )
├─ app/
│ └─ streamlit_app.py # upload → outputs + advice + chat
└─ infra/
├─ main.tf
├─ variables.tf
└─ outputs.tf

```


## 🚀 Roadmap
1. ✅ Repo skeleton + Food-101 subset → EfficientNet baseline  
2. Waste estimator (heuristic → OpenMMLab small model)  
3. Agentic ChefBot (LangChain + tools + FAISS on `kb.csv`)  
4. API (`/predict`, `/chat`) + Streamlit (upload + chat)  
5. Terraform (S3, Lambda, API GW, Glue, Athena) + CI  
6. Polish (README GIF, metrics, limitations, costs)  

---

## ✅ Acceptance (demo-ready)
- Classifier ≥85% top-1 (8–10 classes) + confusion matrix  
- Waste % visible + mask overlay  
- ChefBot answers 5 realistic chef questions (3 tips + CO₂ context)  
- `terraform apply` → public API responding to `/predict` and `/chat`  
- Athena query returns last 10 predictions