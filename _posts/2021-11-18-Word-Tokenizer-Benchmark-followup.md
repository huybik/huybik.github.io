---
layout: post
title: Short update on benchmarking popular Vietnamese NLP tools 
---

This is a follow up post to my previous review. Due to new versions of these tools being out that can potentially change speed and accuracy significantly. Most notable change is from Underthesea, with vastly increase segmentation speed as you will see in my benchmark below. We still use UD_Vietnamese-VTB dataset for this benchmark which comes with it's limitations, so take the accuracy results with a grain of salt.

For this benchmark we use these configurations below. The code for the benchmark can be found at [Tokenizers Benchmark source code](https://github.com/huybik/Tokenizers-benchmark)

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
<p align="center">
<img class="figure" src='https://github.com/huybik/Tokenizers-benchmark/blob/16f303cbe68142a4f7eaabeb4de2edda7bbd8b65/images/runtime.png?raw=true'>
<p align = "center"> Annotate speed </p>
</p>

Word annotation is a multi-step pipeline, it starts with word segmentation then doing POS Tagging (Parts of Speech Tagging), NER (Name Entity Recognition) and more. Unfortunately, our dataset doesn't have groundtruth for NER, but we still do NER processing mainly to compare annotation speed.

<p align="center">
<img class="figure" src='https://github.com/huybik/Tokenizers-benchmark/blob/16f303cbe68142a4f7eaabeb4de2edda7bbd8b65/images/tokenize.png?raw=true'>
<p align = "center"> Tokenize speed </p>
</p>

If we do word segmentation alone, the improvement between version 1.3.3 and 1.3.4 is huge at 1.5 times the speed, very impressive for a pure python implementation. Noted ViSpacy result is missing as they don't have separate function for word segmentation. Speed alone doesn't make a good tokenizer, but it makes processing milions of words a lot less painful. For accuracy comparision, while it heavily depends on the dataset in use, it does paint an overal picture on quality of a tokenizer.  

<p align="center">
<img class="figure" src='https://github.com/huybik/Tokenizers-benchmark/blob/16f303cbe68142a4f7eaabeb4de2edda7bbd8b65/images/accuracy.png?raw=true'>
<p align = "center"> Word segmentation and POS tag accuracy </p>
</p>

There are other tokenizers out there with python support but due to my windows machine I didn't have the means to build from source right now. Coccoc tokenizer in particular promise very high segmentation speed by using C++ for the core and python wrapper for end user convenient, a good candidate for running benchmark when I have spare time to get Linux environment ready.

