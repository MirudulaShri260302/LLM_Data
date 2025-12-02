#  LLM Data Pipeline – Labs 1 & 2

This repository contains two hands-on labs demonstrating how to build **data preprocessing pipelines for Large Language Models (LLMs)**.  
Both labs prepare data for causal language modeling (GPT-style training), using two different approaches:

- **Lab 1:** Classic full-dataset preprocessing (non-streaming)  
- **Lab 2:** Modern streaming-based preprocessing  

---

#  Lab 1 — Classic LM Preprocessing with Custom `<PARA>` Token

##  Objective
Build a traditional LM preprocessing pipeline that:

- Loads the entire WikiText-2 dataset into memory  
- Tokenizes all documents with GPT-2 tokenizer  
- Concatenates everything into one long token stream  
- Splits into fixed-size training blocks (128 tokens)  
- Produces PyTorch batches  

This mirrors early GPT-2 preprocessing.

---

##  Custom Modification
To make this lab unique, a new special token was added:
`<PARA>`
This token is inserted between every document to preserve paragraph or section boundaries.

### ✔ Why this matters

- Helps the model recognize paragraph transitions  
- Mimics real LLM training approaches (e.g., Anthropic, Meta)  
- Demonstrates tokenizer extension & vocabulary modification  
- Distinguishes Lab 1 from Lab 2’s EOS-based segmentation  

### ✔ Example Output
```text
... British footballers .
<PARA><PARA> == International career ==
```

This confirms that the custom token is successfully used between documents.

---

#  Lab 2 — Streaming LM Pipeline with Rolling Buffer + EOS Padding

## Objective

Build a streaming-based LM preprocessing pipeline that **does not load the full dataset into memory**.

The pipeline:

- Loads WikiText-2 in streaming mode  
- Tokenizes text lazily, sample by sample  
- Uses a rolling buffer to concatenate tokens  
- Splits into fixed-length blocks (128 tokens)  
- Creates clean PyTorch LM batches  

This matches preprocessing workflows used in large-scale LLM training (e.g., GPT-3, LLaMA).

---

##  Custom Modifications

### **1. Add EOS token between streaming documents**
Explicitly marks document boundaries during streaming.

### **2. Pad the final leftover block**
Ensures **all** training sequences remain exactly 128 tokens long, even at the end of the dataset.

---

### ✔ Example Output (Lab 2)
```text
<|endoftext|> = Valkyria Chronicles III =
<|endoftext|><|endoftext|> Senjō no Valkyria 3 : Unrecorded Chronicles ...
```





