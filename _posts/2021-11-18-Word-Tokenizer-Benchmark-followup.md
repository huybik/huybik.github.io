---
layout: post
title: Short update on benchmarking popular Vietnamese NLP tools 
---

This is a follow up post to my previous review. Due to new versions of these tools being out that change speed and accuracy significantly. We still use UD_Vietnamese-VTB dataset for this benchmark which comes with it's limitations, so take the accuracy results with a grain of salt. 

For this benchmark we use these configurations below. The code for the benchmark can be found at [Tokenizers-benchmark](https://github.com/huybik/Tokenizers-benchmark)

```
Spec: Core i7 8550U 4 cores 8 threads
Jupyter notebook on windows 11
python 3.9.7

DATASET
https://github.com/UniversalDependencies/UD_Vietnamese-VTB 1400 sentences

REQUIREMENTS
conll==4.4.1
ViSpacy==0.0.1 https://gitlab.com/trungtv/vi_spacy/-/raw/master/vi_core_news_lg/dist/vi_core_news_lg-0.0.1.tar.gz
Underthesea==1.3.4a1
Vncorenlp==1.0.3 
Java
```

<img src='images/accuracy.png' width=400>

<img src='images/runtime.png' width=400>
