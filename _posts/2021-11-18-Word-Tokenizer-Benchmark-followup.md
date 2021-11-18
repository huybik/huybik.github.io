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
<p align="left">
<img src='https://github.com/huybik/Tokenizers-benchmark/blob/1b8662504e5c515303b045a9a28b518dec237a11/images/runtime.png?raw=true' width=500>
</p>

Overall we see huge improvement on speed for Underthesea with their latest update, very impressive for a pure python implementation. Accuracy on the other hand is not changing much.

<p align="left">
<img src='https://github.com/huybik/Tokenizers-benchmark/blob/1b8662504e5c515303b045a9a28b518dec237a11/images/accuracy.png?raw=true' width=500>
</p>





