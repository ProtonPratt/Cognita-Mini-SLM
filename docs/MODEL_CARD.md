---
license: cc-by-nc-4.0
language:
- en
- or
- doi
library_name: transformers
pipeline_tag: text-generation
tags:
- odia
- dogri
- multilingual
- trained-from-scratch
- qwen3
datasets:
- wikimedia/wikipedia
- HuggingFaceFW/fineweb
- ai4bharat/IndicCorpV2
- ai4bharat/BPCC
- OdiaGenAIdata/pre_train_odia_data_processed
- StephennFernandes/ciil_mega_corpus_dogri
- saillab/alpaca-dogri-cleaned
---

# Cognita-Mini-167M

A 167M-parameter decoder-only language model for **English, Odia and Dogri**, pre-trained from random
initialisation on 2.42 billion tokens. The architecture follows the Qwen3 layout; no pre-trained
weights were used, and the tokenizer was trained for this project.

Code, figures and the full write-up: **https://github.com/ProtonPratt/Cognita-Mini-SLM**

| | |
|---|---|
| Parameters | 167,469,952 |
| Layers × width | 12 × 896 |
| Attention | 12 query heads, 4 key/value heads (grouped-query attention) |
| Context window | 4,096 tokens |
| Vocabulary | 32,000-piece SentencePiece BPE shared by Latin, Odia and Devanagari script |
| Training data | 2.42B tokens: English 66.2%, Odia 33.4%, Dogri 0.4% |
| Training | 2 epochs, 4.76B tokens processed, 8.6 h on 2× H100 |
| Validation | loss 3.203, perplexity 24.6 on 24M held-out tokens |

## Usage

```python
from transformers import AutoModelForCausalLM, LlamaTokenizer

repo = "ProtonPrat/Cognita-Mini-167M"
tokenizer = LlamaTokenizer.from_pretrained(repo)
model = AutoModelForCausalLM.from_pretrained(repo)

inputs = tokenizer("ଓଡ଼ିଶା ଭାରତର ଏକ ରାଜ୍ୟ ଯାହା", return_tensors="pt")
output = model.generate(**inputs, max_new_tokens=80, do_sample=True, temperature=0.7,
                        top_p=0.8, repetition_penalty=1.1, no_repeat_ngram_size=3)
print(tokenizer.decode(output[0], skip_special_tokens=True))
```

Requires `transformers>=4.56,<5` and `sentencepiece`. Load the tokenizer with `LlamaTokenizer`.

This is a plain language model: it continues text. For question-style prompts, add the
[instruction adapter](https://huggingface.co/ProtonPrat/Cognita-Mini-167M-instruct-lora).

## Training data

| Language | Tokens | Sources |
|---|---:|---|
| English | 1,602,942,297 | Wikipedia (2023-11 dump), FineWeb (CC-MAIN-2025-26) |
| Odia | 807,950,933 | AI4Bharat IndicCorp v2 and BPCC, eleven OdiaGenAI collections |
| Dogri | 10,030,800 | CIIL Dogri corpus, a Dogri Alpaca-style instruction set |

Text was NFC-normalised, stripped of HTML, URLs and control characters, and filtered to lines of more
than five words. Token streams were packed into 4,096-token sequences with no padding.

## Training procedure

AdamW, peak learning rate 3e-4 with 1% warm-up and cosine decay, weight decay 0.1, bfloat16 autocast,
786,432 tokens per update (192 sequences), 6,052 updates. 1% of the packed sequences were held out for
validation; the best checkpoint by validation loss is released.

## Evaluation

| Metric | Value |
|---|---:|
| Validation perplexity (in-distribution, 24M tokens) | 24.6 |
| PIQA validation, two-way likelihood scoring (chance 50%) | 57.2% |

## Limitations

- A small model trained on a small corpus: fluent, but it invents facts. No safety tuning.
- Dogri is 0.4% of the training data and is the weakest of the three languages.
- Validation text comes from the same sources as the training text, so perplexity does not measure
  transfer to new domains.
- Not evaluated for bias. Web and news text carry the biases of their sources.

## License

CC BY-NC 4.0. Research and non-commercial use.

## Citation

```bibtex
@misc{jena2025cognitamini,
  author = {Pratyush Jena},
  title  = {Cognita-Mini: a 167M-parameter English, Odia and Dogri language model trained from scratch},
  year   = {2025},
  url    = {https://github.com/ProtonPratt/Cognita-Mini-SLM}
}
```

Please also cite the data this model was trained on: Wikipedia, FineWeb, IndicCorp v2, BPCC, the
OdiaGenAI Large Odia LLM Corpus, the CIIL Dogri corpus and the TaCo Dogri Alpaca set. Every source, with
its licence and a BibTeX entry, is listed in
[docs/CITATIONS.md](https://github.com/ProtonPratt/Cognita-Mini-SLM/blob/main/docs/CITATIONS.md).
