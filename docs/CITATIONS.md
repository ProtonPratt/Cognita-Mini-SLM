# Citations and sources

Cognita-Mini is trained from scratch, but it is built entirely from other people's data, models, methods
and software. This page lists every one of them, with the exact identifier the code uses, where it is
used, its licence, and a BibTeX entry. If you use Cognita-Mini, please cite the works below that your
use depends on as well as this repository.

Contents: [Citing Cognita-Mini](#citing-cognita-mini) · [Pre-training data](#pre-training-data) ·
[Instruction-tuning and evaluation data](#instruction-tuning-and-evaluation-data) ·
[Models](#models) · [Architecture and training methods](#architecture-and-training-methods) ·
[Software](#software) · [BibTeX](#bibtex)

## Citing Cognita-Mini

```bibtex
@misc{jena2025cognitamini,
  author = {Pratyush Jena},
  title  = {Cognita-Mini: a 167M-parameter English, Odia and Dogri language model trained from scratch},
  year   = {2025},
  url    = {https://github.com/ProtonPratt/Cognita-Mini-SLM}
}
```

GitHub also offers this entry through "Cite this repository", generated from
[`CITATION.cff`](../CITATION.cff).

## Pre-training data

| Language | Dataset | Identifier used | Code | Licence | Cite |
|---|---|---|---|---|---|
| English | Wikipedia | [`wikimedia/wikipedia`](https://huggingface.co/datasets/wikimedia/wikipedia), config `20231101.en` | [`data/download_english.py`](../data/download_english.py) | CC BY-SA 3.0, GFDL | `wikidump` |
| English | FineWeb | [`HuggingFaceFW/fineweb`](https://huggingface.co/datasets/HuggingFaceFW/fineweb), config `CC-MAIN-2025-26` | [`data/download_english.py`](../data/download_english.py) | ODC-By 1.0 | `penedo2024fineweb` |
| Odia | IndicCorp v2 | [`ai4bharat/IndicCorpV2`](https://huggingface.co/datasets/ai4bharat/IndicCorpV2), split `ory_Orya` | [`data/text_to_shards.py`](../data/text_to_shards.py) | CC0 | `doddapaneni2023indiccorp` |
| Odia | BPCC | [`ai4bharat/BPCC`](https://huggingface.co/datasets/ai4bharat/BPCC), `nllb-filtered`, Odia side of `ory_Orya` | [`data/build_odia_corpus.py`](../data/build_odia_corpus.py) | CC0 (mined data) | `gala2023indictrans2` |
| Odia | Large Odia LLM Corpus (OdiaGenAI), 11 source subsets | [`OdiaGenAIdata/pre_train_odia_data_processed`](https://huggingface.co/datasets/OdiaGenAIdata/pre_train_odia_data_processed) | [`data/build_odia_corpus.py`](../data/build_odia_corpus.py) | CC BY-NC-SA 4.0 | `parida2024odiacorpus` |
| Dogri | CIIL Dogri corpus | [`StephennFernandes/ciil_mega_corpus_dogri`](https://huggingface.co/datasets/StephennFernandes/ciil_mega_corpus_dogri) | [`data/dataset_to_shards.py`](../data/dataset_to_shards.py) | not stated | Central Institute of Indian Languages, Mysuru, via the Hub mirror |
| Dogri | Alpaca-52K translated into Dogri | [`saillab/alpaca-dogri-cleaned`](https://huggingface.co/datasets/saillab/alpaca-dogri-cleaned) | [`data/dataset_to_shards.py`](../data/dataset_to_shards.py) | CC BY-NC | `upadhayay2024taco`, `taori2023alpaca` |

**The OdiaGenAI subsets** used are `culturax`, `indicqa`, `odiencorp`, `oscar`, `paraphrasing`, `pmo`,
`samanantar`, `indicSentiment`, `varta`, `wikipedia` and `xp3` (the local folder names in
[`data/build_odia_corpus.py`](../data/build_odia_corpus.py) differ slightly). Each subset is OdiaGenAI's
Odia extract of an upstream corpus. The OdiaGenAI card asks to be cited as the source. If you use any
subset on its own, please also cite its upstream work: CulturaX (`nguyen2024culturax`), OSCAR
(`ortizsuarez2019oscar`), Samanantar (`ramesh2022samanantar`), Varta (`aralikatte2023varta`), xP3
(`muennighoff2023xp3`), OdiEnCorp 2.0 (`parida2020odiencorp`), IndicQA and IndicSentiment
(`doddapaneni2023indiccorp`), and Wikipedia (`wikidump`).

**The Dogri Alpaca set** is a Google Translate rendering of Stanford Alpaca-52K, released with the TaCo
paper. Its card restricts it to academic and research use.

## Instruction-tuning and evaluation data

| Dataset | Identifier used | Role | Code | Licence | Cite |
|---|---|---|---|---|---|
| mCSQA, English | [`yusuke1997/mCSQA`](https://huggingface.co/datasets/yusuke1997/mCSQA), config `en` | QA instructions | [`finetune/build_qa_dataset.py`](../finetune/build_qa_dataset.py) | OpenAI terms of use (`other`) | `sakai2024mcsqa` |
| CommonsenseQA | [`tau/commonsense_qa`](https://huggingface.co/datasets/tau/commonsense_qa) | QA instructions | [`finetune/build_qa_dataset.py`](../finetune/build_qa_dataset.py) | MIT | `talmor2019commonsenseqa` |
| Cosmos QA | [`wilburOne/cosmosqa`](https://github.com/wilburOne/cosmosqa), `data/*.csv` | QA instructions | [`finetune/build_qa_dataset.py`](../finetune/build_qa_dataset.py) | CC BY 4.0 | `huang2019cosmosqa` |
| PIQA | [`ybisk/ybisk.github.io`](https://github.com/ybisk/ybisk.github.io/tree/master/piqa), `piqa/data` | QA instructions; validation split for evaluation | [`finetune/build_qa_dataset.py`](../finetune/build_qa_dataset.py), [`eval/piqa_accuracy.py`](../eval/piqa_accuracy.py) | AFL 3.0 | `bisk2020piqa` |
| RefinedWeb | [`tiiuae/falcon-refinedweb`](https://huggingface.co/datasets/tiiuae/falcon-refinedweb) | English sentences for character counting | [`finetune/build_char_count_dataset.py`](../finetune/build_char_count_dataset.py) | ODC-By 1.0 | `penedo2023refinedweb` |

The Odia and Kashmiri (Devanagari) QA sets are machine translations of the English set made with
NLLB-200 (see [Models](#models)). The Odia and Devanagari character-counting sets are synthetic and have
no source.

## Models

No pre-trained weights are loaded into Cognita-Mini. These models are used as tools around it.

| Model | Identifier used | Role | Code | Licence | Cite |
|---|---|---|---|---|---|
| NLLB-200 3.3B | [`facebook/nllb-200-3.3B`](https://huggingface.co/facebook/nllb-200-3.3B) | Translates the English QA set into Odia (`ory_Orya`) and Kashmiri (`kas_Deva`) | [`finetune/translate_nllb.py`](../finetune/translate_nllb.py) | CC BY-NC 4.0 | `nllb2022` |
| Qwen2-7B tokenizer | [`Qwen/Qwen2-7B`](https://huggingface.co/Qwen/Qwen2-7B) | Counts tokens to set download budgets, before the project tokenizer existed | [`data/download_english.py`](../data/download_english.py), [`data/text_to_shards.py`](../data/text_to_shards.py) | Apache 2.0 | `yang2024qwen2` |
| Qwen3 tokenizer | [`Qwen/Qwen3-0.6B`](https://huggingface.co/Qwen/Qwen3-0.6B) | Baseline in the tokens-per-word comparison | [`tokenizer/analyze_tokenizer.py`](../tokenizer/analyze_tokenizer.py) | Apache 2.0 | `yang2025qwen3` |
| Qwen3 architecture | `Qwen3Config`, `Qwen3ForCausalLM` in Transformers | Model layout, randomly initialised | [`pretrain/config.py`](../pretrain/config.py) | Apache 2.0 | `yang2025qwen3` |

## Architecture and training methods

| Component | Where | Cite |
|---|---|---|
| Decoder-only transformer | whole model | `vaswani2017attention` |
| Grouped-query attention | 12 query heads share 4 key/value heads | `ainslie2023gqa` |
| Rotary position embeddings | every attention layer | `su2024roformer` |
| SwiGLU feed-forward | every block | `shazeer2020glu` |
| RMSNorm | pre-norm, and QK-norm | `zhang2019rmsnorm` |
| QK-norm | normalised queries and keys | `henry2020qknorm` |
| Byte-pair encoding | tokenizer | `sennrich2016bpe` |
| SentencePiece | tokenizer training and inference | `kudo2018sentencepiece` |
| AdamW | pre-training and LoRA | `loshchilov2019adamw` |
| Cosine learning-rate decay | pre-training and LoRA | `loshchilov2017sgdr` |
| LoRA | instruction tuning | `hu2022lora` |
| Alpaca prompt format (`### Instruction:` / `### Response:`) | instruction tuning and inference | `taori2023alpaca` |

## Software

| Library | Version in the released run | Cite |
|---|---|---|
| [PyTorch](https://pytorch.org) | 2.8.0 | `paszke2019pytorch` |
| [Transformers](https://github.com/huggingface/transformers) | 4.56.1 | `wolf2020transformers` |
| [Datasets](https://github.com/huggingface/datasets) | 4.1.0 | `lhoest2021datasets` |
| [PEFT](https://github.com/huggingface/peft) | 0.17.1 | `mangrulkar2022peft` |
| [Accelerate](https://github.com/huggingface/accelerate) | 1.10.1 | `gugger2022accelerate` |
| [SentencePiece](https://github.com/google/sentencepiece) | 0.2.1 | `kudo2018sentencepiece` |

NumPy, PyArrow, Matplotlib, tqdm and the Hugging Face Hub client are also used; see
[`requirements.txt`](../requirements.txt).

## BibTeX

```bibtex
% ---------------------------------------------------------------- Pre-training data

@misc{wikidump,
  author       = {{Wikimedia Foundation}},
  title        = {Wikimedia Downloads},
  howpublished = {\url{https://dumps.wikimedia.org}}
}

@inproceedings{penedo2024fineweb,
  title     = {The {FineWeb} Datasets: Decanting the Web for the Finest Text Data at Scale},
  author    = {Guilherme Penedo and Hynek Kydl{\'\i}{\v{c}}ek and Loubna Ben allal and Anton Lozhkov and
               Margaret Mitchell and Colin Raffel and Leandro Von Werra and Thomas Wolf},
  booktitle = {The Thirty-eighth Conference on Neural Information Processing Systems Datasets and Benchmarks Track},
  year      = {2024},
  url       = {https://arxiv.org/abs/2406.17557}
}

@inproceedings{doddapaneni2023indiccorp,
  title     = {Towards Leaving No {I}ndic Language Behind: Building Monolingual Corpora, Benchmark and
               Models for {I}ndic Languages},
  author    = {Doddapaneni, Sumanth and Aralikatte, Rahul and Ramesh, Gowtham and Goyal, Shreya and
               Khapra, Mitesh M. and Kunchukuttan, Anoop and Kumar, Pratyush},
  booktitle = {Proceedings of the 61st Annual Meeting of the Association for Computational Linguistics
               (Volume 1: Long Papers)},
  year      = {2023},
  address   = {Toronto, Canada},
  publisher = {Association for Computational Linguistics},
  pages     = {12402--12426},
  doi       = {10.18653/v1/2023.acl-long.693}
}

@article{gala2023indictrans2,
  title   = {{IndicTrans2}: Towards High-Quality and Accessible Machine Translation Models for all 22
             Scheduled {I}ndian Languages},
  author  = {Jay Gala and Pranjal A Chitale and A K Raghavan and Varun Gumma and Sumanth Doddapaneni and
             Aswanth Kumar M and Janki Atul Nawale and Anupama Sujatha and Ratish Puduppully and
             Vivek Raghavan and Pratyush Kumar and Mitesh M Khapra and Raj Dabre and Anoop Kunchukuttan},
  journal = {Transactions on Machine Learning Research},
  issn    = {2835-8856},
  year    = {2023},
  url     = {https://openreview.net/forum?id=vfT4YuzAYA}
}

@misc{parida2024odiacorpus,
  author       = {Shantipriya Parida and Sambit Sekhar and Debasish Dhal and Pritiprava Mishra and
                  Suman Kumar Maharana and Purushottam Kumar and Priyabrata Jena and Guneet Singh Kohli and
                  Kalyanamalini Sahoo},
  title        = {Large {O}dia {LLM} Corpus},
  year         = {2024},
  publisher    = {Hugging Face},
  howpublished = {\url{https://huggingface.co/datasets/OdiaGenAIdata/pre_train_odia_data_processed}}
}

@inproceedings{upadhayay2024taco,
  title     = {{TaCo}: Enhancing Cross-Lingual Transfer for Low-Resource Languages in {LLM}s through
               Translation-Assisted Chain-of-Thought Processes},
  author    = {Bibek Upadhayay and Vahid Behzadan},
  booktitle = {5th Workshop on Practical ML for Limited/Low Resource Settings, ICLR},
  year      = {2024},
  url       = {https://openreview.net/forum?id=02MLWBj8HP}
}

@misc{taori2023alpaca,
  author       = {Rohan Taori and Ishaan Gulrajani and Tianyi Zhang and Yann Dubois and Xuechen Li and
                  Carlos Guestrin and Percy Liang and Tatsunori B. Hashimoto},
  title        = {Stanford Alpaca: An Instruction-following {LLaMA} model},
  year         = {2023},
  publisher    = {GitHub},
  howpublished = {\url{https://github.com/tatsu-lab/stanford_alpaca}}
}

% Upstream sources of the OdiaGenAI subsets

@inproceedings{nguyen2024culturax,
  title     = {{CulturaX}: A Cleaned, Enormous, and Multilingual Dataset for Large Language Models in
               167 Languages},
  author    = {Nguyen, Thuat and Van Nguyen, Chien and Lai, Viet Dac and Man, Hieu and Ngo, Nghia Trung and
               Dernoncourt, Franck and Rossi, Ryan A. and Nguyen, Thien Huu},
  booktitle = {Proceedings of the 2024 Joint International Conference on Computational Linguistics,
               Language Resources and Evaluation (LREC-COLING 2024)},
  year      = {2024},
  url       = {https://arxiv.org/abs/2309.09400}
}

@inproceedings{ortizsuarez2019oscar,
  title     = {Asynchronous Pipelines for Processing Huge Corpora on Medium to Low Resource Infrastructures},
  author    = {Ortiz Su{\'a}rez, Pedro Javier and Sagot, Beno{\^\i}t and Romary, Laurent},
  booktitle = {Proceedings of the Workshop on Challenges in the Management of Large Corpora (CMLC-7)},
  year      = {2019},
  doi       = {10.14618/ids-pub-9021}
}

@article{ramesh2022samanantar,
  title   = {Samanantar: The Largest Publicly Available Parallel Corpora Collection for 11 {I}ndic Languages},
  author  = {Ramesh, Gowtham and Doddapaneni, Sumanth and Bheemaraj, Aravinth and others},
  journal = {Transactions of the Association for Computational Linguistics},
  volume  = {10},
  pages   = {145--162},
  year    = {2022},
  doi     = {10.1162/tacl_a_00452}
}

@inproceedings{aralikatte2023varta,
  title     = {V{\=a}rta: A Large-Scale Headline-Generation Dataset for {I}ndic Languages},
  author    = {Aralikatte, Rahul and Cheng, Ziling and Doddapaneni, Sumanth and Cheung, Jackie Chi Kit},
  booktitle = {Findings of the Association for Computational Linguistics: ACL 2023},
  year      = {2023},
  url       = {https://arxiv.org/abs/2305.05858}
}

@inproceedings{muennighoff2023xp3,
  title     = {Crosslingual Generalization through Multitask Finetuning},
  author    = {Muennighoff, Niklas and Wang, Thomas and Sutawika, Lintang and others},
  booktitle = {Proceedings of the 61st Annual Meeting of the Association for Computational Linguistics
               (Volume 1: Long Papers)},
  year      = {2023},
  url       = {https://arxiv.org/abs/2211.01786}
}

@inproceedings{parida2020odiencorp,
  title     = {{OdiEnCorp} 2.0: {O}dia-{E}nglish Parallel Corpus for Machine Translation},
  author    = {Parida, Shantipriya and Dash, Satya Ranjan and Bojar, Ond{\v{r}}ej and Motlicek, Petr and
               Pattnaik, Priyanka and Mallick, Debasish Kumar},
  booktitle = {Proceedings of the WILDRE5 -- 5th Workshop on Indian Language Data: Resources and Evaluation},
  year      = {2020},
  url       = {https://aclanthology.org/2020.wildre-1.3}
}

% ---------------------------------------------------------------- Instruction-tuning and evaluation data

@inproceedings{sakai2024mcsqa,
  title     = {{mCSQA}: Multilingual Commonsense Reasoning Dataset with Unified Creation Strategy by
               Language Models and Humans},
  author    = {Sakai, Yusuke and Kamigaito, Hidetaka and Watanabe, Taro},
  booktitle = {Findings of the Association for Computational Linguistics: ACL 2024},
  year      = {2024},
  address   = {Bangkok, Thailand},
  publisher = {Association for Computational Linguistics},
  pages     = {14182--14214},
  doi       = {10.18653/v1/2024.findings-acl.844}
}

@inproceedings{talmor2019commonsenseqa,
  title     = {{C}ommonsense{QA}: A Question Answering Challenge Targeting Commonsense Knowledge},
  author    = {Talmor, Alon and Herzig, Jonathan and Lourie, Nicholas and Berant, Jonathan},
  booktitle = {Proceedings of the 2019 Conference of the North {A}merican Chapter of the Association for
               Computational Linguistics: Human Language Technologies, Volume 1 (Long and Short Papers)},
  year      = {2019},
  address   = {Minneapolis, Minnesota},
  publisher = {Association for Computational Linguistics},
  pages     = {4149--4158},
  doi       = {10.18653/v1/N19-1421}
}

@inproceedings{huang2019cosmosqa,
  title     = {Cosmos {QA}: Machine Reading Comprehension with Contextual Commonsense Reasoning},
  author    = {Huang, Lifu and Le Bras, Ronan and Bhagavatula, Chandra and Choi, Yejin},
  booktitle = {Proceedings of the 2019 Conference on Empirical Methods in Natural Language Processing and
               the 9th International Joint Conference on Natural Language Processing (EMNLP-IJCNLP)},
  year      = {2019},
  address   = {Hong Kong, China},
  publisher = {Association for Computational Linguistics},
  pages     = {2391--2401},
  doi       = {10.18653/v1/D19-1243}
}

@inproceedings{bisk2020piqa,
  title     = {{PIQA}: Reasoning about Physical Commonsense in Natural Language},
  author    = {Bisk, Yonatan and Zellers, Rowan and Le Bras, Ronan and Gao, Jianfeng and Choi, Yejin},
  booktitle = {Proceedings of the AAAI Conference on Artificial Intelligence},
  volume    = {34},
  number    = {05},
  pages     = {7432--7439},
  year      = {2020},
  doi       = {10.1609/aaai.v34i05.6239}
}

@inproceedings{penedo2023refinedweb,
  title     = {The {RefinedWeb} Dataset for {F}alcon {LLM}: Outperforming Curated Corpora with Web Data,
               and Web Data Only},
  author    = {Penedo, Guilherme and Malartic, Quentin and Hesslow, Daniel and Cojocaru, Ruxandra and
               Cappelli, Alessandro and Alobeidli, Hamza and Pannier, Baptiste and Almazrouei, Ebtesam and
               Launay, Julien},
  booktitle = {Advances in Neural Information Processing Systems 36 (Datasets and Benchmarks Track)},
  year      = {2023},
  url       = {https://arxiv.org/abs/2306.01116}
}

% ---------------------------------------------------------------- Models

@article{nllb2022,
  title   = {No Language Left Behind: Scaling Human-Centered Machine Translation},
  author  = {{NLLB Team} and Costa-juss{\`a}, Marta R. and Cross, James and {\c{C}}elebi, Onur and
             Elbayad, Maha and Heafield, Kenneth and Heffernan, Kevin and Kalbassi, Elahe and Lam, Janice and
             Licht, Daniel and Maillard, Jean and Sun, Anna and Wang, Skyler and Wenzek, Guillaume and
             Youngblood, Al and others},
  journal = {arXiv preprint arXiv:2207.04672},
  year    = {2022}
}

@article{yang2024qwen2,
  title   = {{Qwen2} Technical Report},
  author  = {Yang, An and others},
  journal = {arXiv preprint arXiv:2407.10671},
  year    = {2024}
}

@article{yang2025qwen3,
  title   = {{Qwen3} Technical Report},
  author  = {Yang, An and others},
  journal = {arXiv preprint arXiv:2505.09388},
  year    = {2025}
}

% ---------------------------------------------------------------- Architecture and training methods

@inproceedings{vaswani2017attention,
  title     = {Attention Is All You Need},
  author    = {Vaswani, Ashish and Shazeer, Noam and Parmar, Niki and Uszkoreit, Jakob and Jones, Llion and
               Gomez, Aidan N. and Kaiser, {\L}ukasz and Polosukhin, Illia},
  booktitle = {Advances in Neural Information Processing Systems 30},
  year      = {2017}
}

@inproceedings{ainslie2023gqa,
  title     = {{GQA}: Training Generalized Multi-Query Transformer Models from Multi-Head Checkpoints},
  author    = {Ainslie, Joshua and Lee-Thorp, James and de Jong, Michiel and Zemlyanskiy, Yury and
               Lebr{\'o}n, Federico and Sanghai, Sumit},
  booktitle = {Proceedings of the 2023 Conference on Empirical Methods in Natural Language Processing},
  year      = {2023},
  pages     = {4895--4901},
  doi       = {10.18653/v1/2023.emnlp-main.298}
}

@article{su2024roformer,
  title   = {{RoFormer}: Enhanced Transformer with Rotary Position Embedding},
  author  = {Su, Jianlin and Ahmed, Murtadha and Lu, Yu and Pan, Shengfeng and Bo, Wen and Liu, Yunfeng},
  journal = {Neurocomputing},
  volume  = {568},
  pages   = {127063},
  year    = {2024},
  doi     = {10.1016/j.neucom.2023.127063}
}

@article{shazeer2020glu,
  title   = {{GLU} Variants Improve Transformer},
  author  = {Shazeer, Noam},
  journal = {arXiv preprint arXiv:2002.05202},
  year    = {2020}
}

@inproceedings{zhang2019rmsnorm,
  title     = {Root Mean Square Layer Normalization},
  author    = {Zhang, Biao and Sennrich, Rico},
  booktitle = {Advances in Neural Information Processing Systems 32},
  year      = {2019}
}

@inproceedings{henry2020qknorm,
  title     = {Query-Key Normalization for Transformers},
  author    = {Henry, Alex and Dachapally, Prudhvi Raj and Pawar, Shubham Shantaram and Chen, Yuxuan},
  booktitle = {Findings of the Association for Computational Linguistics: EMNLP 2020},
  year      = {2020},
  pages     = {4246--4253},
  doi       = {10.18653/v1/2020.findings-emnlp.379}
}

@inproceedings{sennrich2016bpe,
  title     = {Neural Machine Translation of Rare Words with Subword Units},
  author    = {Sennrich, Rico and Haddow, Barry and Birch, Alexandra},
  booktitle = {Proceedings of the 54th Annual Meeting of the Association for Computational Linguistics
               (Volume 1: Long Papers)},
  year      = {2016},
  pages     = {1715--1725},
  doi       = {10.18653/v1/P16-1162}
}

@inproceedings{kudo2018sentencepiece,
  title     = {{S}entence{P}iece: A simple and language independent subword tokenizer and detokenizer for
               Neural Text Processing},
  author    = {Kudo, Taku and Richardson, John},
  booktitle = {Proceedings of the 2018 Conference on Empirical Methods in Natural Language Processing:
               System Demonstrations},
  year      = {2018},
  pages     = {66--71},
  doi       = {10.18653/v1/D18-2012}
}

@inproceedings{loshchilov2019adamw,
  title     = {Decoupled Weight Decay Regularization},
  author    = {Loshchilov, Ilya and Hutter, Frank},
  booktitle = {International Conference on Learning Representations},
  year      = {2019}
}

@inproceedings{loshchilov2017sgdr,
  title     = {{SGDR}: Stochastic Gradient Descent with Warm Restarts},
  author    = {Loshchilov, Ilya and Hutter, Frank},
  booktitle = {International Conference on Learning Representations},
  year      = {2017}
}

@inproceedings{hu2022lora,
  title     = {{LoRA}: Low-Rank Adaptation of Large Language Models},
  author    = {Hu, Edward J. and Shen, Yelong and Wallis, Phillip and Allen-Zhu, Zeyuan and Li, Yuanzhi and
               Wang, Shean and Wang, Lu and Chen, Weizhu},
  booktitle = {International Conference on Learning Representations},
  year      = {2022}
}

% ---------------------------------------------------------------- Software

@inproceedings{paszke2019pytorch,
  title     = {{PyTorch}: An Imperative Style, High-Performance Deep Learning Library},
  author    = {Paszke, Adam and Gross, Sam and Massa, Francisco and Lerer, Adam and Bradbury, James and
               Chanan, Gregory and Killeen, Trevor and Lin, Zeming and Gimelshein, Natalia and Antiga, Luca and
               Desmaison, Alban and K{\"o}pf, Andreas and Yang, Edward and DeVito, Zach and Raison, Martin and
               Tejani, Alykhan and Chilamkurthy, Sasank and Steiner, Benoit and Fang, Lu and Bai, Junjie and
               Chintala, Soumith},
  booktitle = {Advances in Neural Information Processing Systems 32},
  year      = {2019}
}

@inproceedings{wolf2020transformers,
  title     = {Transformers: State-of-the-Art Natural Language Processing},
  author    = {Wolf, Thomas and Debut, Lysandre and Sanh, Victor and Chaumond, Julien and Delangue, Clement and
               Moi, Anthony and Cistac, Pierric and Rault, Tim and Louf, R{\'e}mi and Funtowicz, Morgan and
               Davison, Joe and Shleifer, Sam and von Platen, Patrick and Ma, Clara and Jernite, Yacine and
               Plu, Julien and Xu, Canwen and Le Scao, Teven and Gugger, Sylvain and Drame, Mariama and
               Lhoest, Quentin and Rush, Alexander M.},
  booktitle = {Proceedings of the 2020 Conference on Empirical Methods in Natural Language Processing:
               System Demonstrations},
  year      = {2020},
  pages     = {38--45},
  doi       = {10.18653/v1/2020.emnlp-demos.6}
}

@inproceedings{lhoest2021datasets,
  title     = {Datasets: A Community Library for Natural Language Processing},
  author    = {Lhoest, Quentin and others},
  booktitle = {Proceedings of the 2021 Conference on Empirical Methods in Natural Language Processing:
               System Demonstrations},
  year      = {2021},
  pages     = {175--184},
  doi       = {10.18653/v1/2021.emnlp-demo.21}
}

@misc{mangrulkar2022peft,
  title        = {{PEFT}: State-of-the-art Parameter-Efficient Fine-Tuning methods},
  author       = {Sourab Mangrulkar and Sylvain Gugger and Lysandre Debut and Younes Belkada and Sayak Paul and
                  Benjamin Bossan},
  year         = {2022},
  howpublished = {\url{https://github.com/huggingface/peft}}
}

@misc{gugger2022accelerate,
  title        = {Accelerate: Training and inference at scale made simple, efficient and adaptable},
  author       = {Sylvain Gugger and Lysandre Debut and Thomas Wolf and Philipp Schmid and Zachary Mueller and
                  Sourab Mangrulkar and Marc Sun and Benjamin Bossan},
  year         = {2022},
  howpublished = {\url{https://github.com/huggingface/accelerate}}
}
```
