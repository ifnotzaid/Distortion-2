# **Russian Sequence Labeling: POS Tagging & Named Entity Recognition**

This project conducts a comparative analysis of **Statistical Models** (HMM), **Feature-Based Models** (CRF), and **Deep Learning Transformers** (BERT/RoBERTa) on two fundamental NLP tasks for the Russian language: **Part-of-Speech (POS) Tagging** and **Named Entity Recognition (NER)**.

## **📂 Datasets Used**

### **A. Part-of-Speech Tagging: Universal Dependencies (SynTagRus)**

For the POS tagging task, we utilized the **Universal Dependencies (UD) v2.15** corpus, specifically the **Russian SynTagRus** treebank.

* **Source:** [Universal Dependencies \- Russian SynTagRus](https://github.com/UniversalDependencies/UD_Russian-SynTagRus)  
* **Description:** SynTagRus is a "Gold Standard" corpus, manually annotated and human-corrected. It contains news, non-fiction, and fiction prose.  
* **Format:** .conllu (CoNLL-U Format).  
* **Target Labels:** 17 Universal POS Tags (UPOS), including NOUN, VERB, ADJ, PROPN, etc.  
* **Challenge:** Russian is a highly **morphologically rich** language (fusional). A single noun can have 12+ forms based on case and number. This dataset tests a model's ability to analyze word endings (suffixes).

### **B. Named Entity Recognition: WikiANN (PAN-X)**

For the NER task, we utilized the **WikiANN** (Russian subset) dataset.

* **Source:** [Hugging Face WikiANN](https://www.google.com/search?q=https://huggingface.co/datasets/wikiann)  
* **Description:** A "Silver Standard" dataset automatically created by scraping Wikipedia articles. It specifically targets 3 entity types.  
* **Target Labels (BIO Scheme):**  
  * PER: Persons (e.g., Elon Musk)  
  * ORG: Organizations (e.g., Google, Gazprom)  
  * LOC: Locations (e.g., Moscow, Paris)  
* **Challenge:** Because this data is scraped from Wikipedia, it contains many **Titles** and **Lists** rather than full grammatical sentences. This introduced a specific "domain bias" where models struggled to distinguish verbs from proper nouns in sentence structures.