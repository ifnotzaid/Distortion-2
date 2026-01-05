Russian Sequence Labeling: A Comparative Analysis of POS Tagging & NER

📜 Project Overview

This repository contains a comprehensive study of Sequence Labeling tasks for the Russian language. We implemented and compared models spanning three decades of NLP history—from statistical baselines to state-of-the-art Transformers—to solve two fundamental problems:

Part-of-Speech (POS) Tagging: Assigning grammatical roles (Noun, Verb, etc.) to words.

Named Entity Recognition (NER): Extracting entities (Persons, Organizations, Locations) from text.

The primary goal was to evaluate how different architectures handle the specific challenges of Russian Morphology (highly fusional) and Semantic Ambiguity.

📂 Datasets

1. POS Tagging: Universal Dependencies (SynTagRus)

Source: Universal Dependencies v2.15 - Russian SynTagRus

Size: ~24,000 training sentences, ~8,800 test sentences.

Labels: 17 Universal POS Tags (UPOS).

Linguistic Challenge: Russian words are highly inflected. A single root word (e.g., Кот - cat) can have over 12 different forms (Кота, Коту, Котом...). Models must generalize to unseen word forms rather than memorizing vocabulary.

2. NER: WikiANN (PAN-X)

Source: Hugging Face WikiANN (Russian subset)

Size: 20,000 training sentences, 10,000 validation sentences.

Labels: BIO-Scheme (B-PER, I-PER, B-ORG, I-ORG, B-LOC, I-LOC, O).

Linguistic Challenge: The dataset consists largely of Wikipedia titles and article summaries. This introduces a Domain Bias where models may struggle to distinguish proper nouns from sentence-initial words or fail to recognize verbs in standard grammatical sentences.

🧠 Methodology & Models

We implemented 7 distinct architectures to trace the evolution of NLP performance.

A. Baselines & Statistical Models

Unigram Tagger (POS): A frequency-based baseline. Assigns the most frequent tag seen in training to every word. Serves as a control for context-blind performance.

Hidden Markov Model (HMM): A generative probabilistic model. It calculates the probability of a tag sequence using Emission ($P(word|tag)$) and Transition ($P(tag|prev\_tag)$) probabilities.

B. Feature-Based & Engineering Models

Conditional Random Field (CRF): A discriminative model optimized for sequence prediction.

Feature Engineering: We manually implemented extraction rules, including Suffix Analysis (last 2-3 characters) to capture Russian morphology, Capitalization checks for NER, and window-based context features.

spaCy (CNN): Utilized the ru_core_news_sm pipeline, which employs a Transition-Based Parser with Convolutional Neural Networks (CNN).

C. Deep Learning & Transformers

BiLSTM (RNN): A Bidirectional LSTM trained from scratch (PyTorch). Used to demonstrate the limitations of deep learning when training on small datasets without pre-trained embeddings.

XLM-RoBERTa (POS): A massive multilingual Transformer pre-trained on 2.5TB of data. Used in an inference-only capacity to establish a State-of-the-Art (SOTA) ceiling for POS tagging.

RuBERT (NER): A BERT-based model pre-trained specifically on the Russian language (DeepPavlov/rubert-base-cased). We fine-tuned this model for 3 epochs on the WikiANN dataset.

📊 Results & Analysis

Task 1: POS Tagging Results

Comparison on the SynTagRus Test Set

Model

Accuracy

Comparison

Unigram

83.87%

Baseline. Failed on ambiguous words.

HMM

87.38%

Improved context but failed on OOV words.

BiLSTM

~80.14%

Suffered from lack of pre-training (vocabulary sparsity).

spaCy

~94.00%

Strong industrial performance.

CRF

96.43%

Winner (Tie). Manual suffix features solved the morphology problem.

RoBERTa

96.42%

Winner (Tie). Deep semantic understanding matched explicit rules.

🔍 Qualitative Findings (POS)

The "Slang" Test: On the neologism "Zaguglil" (Googled), statistical models failed. The CRF succeeded because it recognized the verbal suffix -il.

The "Homonym" Test: On the sentence "Mother started to bake (pech) pies... stood a stove (pech)", the RoBERTa model was the only one to correctly distinguish the Verb from the Noun using self-attention.

Task 2: NER Results

F1-Score on WikiANN Test Set

Model

F1-Score

Comparison

HMM

73.91%

Failed. Relied on memory; catastrophic failure on unseen names ("Zombie Name" effect).

CRF

85.13%

Good. High score on test set, but overfit to Wikipedia title structures.

RuBERT

91.17%

SOTA. The only model to understand complex sentence structures.

🔍 Qualitative Findings (NER)

The "Elon Musk" Test: Input: "Elon Musk bought Twitter".

HMM: Tagged the entire sentence as a Person (transition error).

CRF: Tagged "bought" as part of the Organization (mistaking the verb for a connector like "Bank of America").

RuBERT: Correctly identified "bought" as a verb (O), separating the Person (PER) from the Organization (ORG).

The "Ambiguity" Test: Input: "Ivan reads a novel" (Roman chitayet roman).

RuBERT successfully distinguished Ivan (Person) from roman (book/noun), while other models struggled with the capitalization ambiguity.

🛠️ Installation

To reproduce these results, clone the repository and install the dependencies:

git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
cd your-repo-name
pip install torch transformers datasets scikit-learn sklearn-crfsuite spacy natasha conllu seqeval evaluate accelerate
python -m spacy download ru_core_news_sm
