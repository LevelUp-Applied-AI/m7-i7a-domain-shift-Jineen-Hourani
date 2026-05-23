
# Module 7 Week A — Integration Task: Domain-Shift Analysis

Apply your fine-tuned classifier (from Lab 7A, hosted on Hugging Face Hub) to the tech / entertainment news corpus and analyze the domain-shift behavior.

Full instructions: see the **Integration Task 7A guide** linked in TalentLMS.

## Quick start

```bash
pip install -r requirements.txt
cp .env.example .env       # then edit MODEL_HUB_ID
make smoke                 # CI substitute model on 5-row fixture
make apply                 # your real model on full 1,033-row tech-news corpus

```

## TODO for learner — fill these in before submitting

* **Hugging Face Hub model URL:** https://huggingface.co/Jineen-Hourani/m7-app-review-sentiment
* **Reproducibility command:**
```bash
cp .env.example .env
# Open .env and set MODEL_HUB_ID=Jineen-Hourani/m7-app-review-sentiment
pip install -r requirements.txt
MODEL_HUB_ID=distilbert-base-uncased-finetuned-sst-2-english CORPUS_PATH=data/tech_news_articles.csv OUTPUT_PATH=predictions.csv python apply.py

```


* **What the model was trained on and why we're applying it here:**
The baseline sequence classification model utilized in this deployment was originally fine-tuned using the specialized AARSynth dataset, which is predominantly comprised of mobile application reviews. This training data is characterized by highly explicit, opinionated, emotional, and localized feedback structures where users explicitly express satisfaction or frustration regarding technical features, bugs, user interfaces, and overall mobile utility. The linguistic tokens embedded within that domain are typically short, dense, and heavily charged with clear polar sentiment signals.
In this integration assignment, we are intentionally subjecting this specialized classifier to a severe domain shift by exposing it to a large corpus of 1,033 tech, entertainment, and digital-culture news articles. Journalistic prose functions under fundamentally different linguistic paradigms compared to consumer feedback reviews. News articles are written in an objective, descriptive, clinical, and formal narrative style. They report on infrastructure updates, regulatory inquiries, product rollouts, and global corporate developments without using overt emotional modifiers or subjective user grading tokens.
By applying our fine-tuned sentiment classifier to this corpus, we expect to analyze and map structural performance degradation caused by keyword-level evaluation biases. Since a standard tech news article naturally contains product-oriented words (such as "update", "patch", "vulnerability", or "deprecate") without expressing actual consumer sentiment, this test allows us to observe how a model behaves when forced to predict categorical boundaries outside its training baseline. It highlights the absolute necessity of contextual domain adaptation and semantic calibration when transferring models from interactive consumer reviews to long-form formal media prose.

## Submission

Open a PR from `integration-7a-domain-shift` into `main`. Paste the PR URL into TalentLMS → Module 7 → Integration Task 7A.

---

## License

This repository is provided for educational use only. See [[LICENSE](https://www.google.com/search?q=LICENSE)](https://www.google.com/search?q=LICENSE) for terms.

You may clone and modify this repository for personal learning and practice, and reference code you wrote here in your professional portfolio. Redistribution outside this course is not permitted.

