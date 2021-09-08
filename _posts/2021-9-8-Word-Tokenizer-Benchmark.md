---
layout: default
title: NLP Benchmarking popular Vietnamese tokenzier
---

I started this when I tried to build a chatbot in Vietnamese for a property company. Natural language processing on Vietnam language is not that different from English due to the fact that they both use alphabetical characters, a dot to end a sentence or semicolons to separate sentences.  The main difference is Vietnam can use 2 or 3 words to form a noun, thus relies heavily on accuracy of words segmentation. The state of annotators for Vietnamese is that they claim to achieves 95% accuracy on large data set, which I think is very good, which includes segmentation, POS and Entity tagging. 


To confirm that, and to see how fast they can segment words, here we going to benchmark 3 popular Vietnamese NLP tool. I use Vietnamese Tree bank data from  https://github.com/UniversalDependencies/UD_Vietnamese-VTB. This data contain more than tagged 800 sentences. These includes word segmentation, pos tag ... but one notable missing is entity tag. The data-set is compatible with Universal Dependency V2, which you can use pre-built tools for processing. For my purpose, I use simple UD python parser from  https://pypi.org/project/conllu/, which helps me parse text data-set into tokens. 

```python
f = open ('UD_Vietnamese-VTB-master/vi_vtb-ud-full.conllu', 'r', encoding='utf-8')
text = f.read()
f.close()
groundtruth = []
sentences = parse(text)
text = ''
sents = []
for tokenlist in sentences:
	tagged = []
	sent = tokenlist.metadata['text']
	for item in tokenlist:
		#print(item['form'],item['xpos'])
		tagged.append([item['form'],item['xpos'],' '])
	text += sent + ' '
	groundtruth.append(tagged)
	sents.append(sent)
	
print(groundtruth)
```

Now all I need is to make simple classes for each of the Tokenizer for Vietnamese I want to benchmark, and they need to have python support. Throughout my research, there are just a few notable works out there for Vietnamese language and all of them support python which is understandable since Python is the most common language for Datascience. These are Underthesea NLP toolkits for python https://github.com/undertheseanlp/underthesea,  VNCoreNLP by Dat Quoc Nguyen implemented in Java with python wrapper https://github.com/vncorenlp/VnCoreNLP, and Vi_SpaCy model that compatible with SpaCy NLP tool  https://github.com/trungtv/vi_spacy. Note that the data-set we use for our benchmark is not being used to train any of the 3 tokenizer tool.

### First we will start with Vi_Spacy. 
Vi_Spacy inherit spacy structure, if you already familiar with spacy it's easy to start working on this.

```python
class Spacy_tokenize:
 
	def __init__(self):
		import spacy
		self.nlp = spacy.load('vi_spacy_model')

		def tokenize(self,text):
		output = []
		doc = self.nlp(text)

		for token in doc:
		    output.append([token.text,token.tag_,''])
		    #print(token.text, token.lemma_, token.tag_, token.pos_, token.dep_,
		    #        token.shape_, token.is_alpha, token.is_stop)

		return output

	def info(self):
		return('PyVi')
```
### Under the sea
Under the sea is written purely python. It good at what it does (tokenize, ner tagging...) and it have many uses. But for word segmentation, run-time is usually long.
```python

class Underthesea_tokenize:
	from underthesea import word_tokenize
		from underthesea import ner
	
	def __init__(self):
		pass
	def tokenize(self,text):
			output = []
			ners = ner(text)
	    for item in ners:
		output.append([item[0],item[1],item[3]])

	    return output

	def info(self):
		return('Underthesea')
```
### VnCoreNLP
VnCoreNLP is written in Java, it requires running separate Java server, and it calls wrapper inside python.
```python
class VncoreNLP_tokenize:
    
	from vncorenlp import VnCoreNLP

	# To perform word segmentation, POS tagging, NER and then dependency parsing
	# annotator = VnCoreNLP("VnCoreNLP-1.1.1.jar", annotators="wseg,pos,ner,parse", max_heap_size='-Xmx2g')
	def __init__(self):
		self.annotator = VnCoreNLP("VnCoreNLP-1.1.1.jar", annotators="wseg,pos,ner,parse", max_heap_size='-Xmx2g')
		def tokenize(self,text):
		output = []
		annotated_text = self.annotator.annotate(text)
		for sent in annotated_text['sentences']:
		    for item in sent:
			output.append([item['form'].replace('_',' '),item['posTag'],item['nerLabel']])
		return output
	def close(self):
		self.annotator.close()
	def info(self):
		return('VnCoreNLP')
```
### Benchmarking 
Those classes above produce tokenized output, which we will feed to to our final function that compares the output and ground-truth extracted from data-set.  We loop through each sentence, use our function tokenize to extract tokens, then compare with ground-truth tokens that sentence. We sum up number of  correct word segment, pos tag and entity tag, then divide by total number of tokens to get final accuracy result. 

```python
# Format for data [sentence, [[word, entity], [word, entity],...]]
for t in (Spacy_tokenize,Underthesea_tokenize,VncoreNLP_tokenize):
	t = t()
	count = 0
	wordcount = 0
	poscount = 0
	sercount = 0


	time = 0
	index = 0

	for sent in sents:

	start = timer()
	predict = t.tokenize(sent)
	time += timer() - start
	count += len(groundtruth[index])

	#print('Predict: ',predict,'Ground-truth: ', groundtruth[index])
	if len(predict) == len(groundtruth[index]):
	    for item,gt in zip(predict,groundtruth[index]):  # item = [word, pos, entity]

		#print(item,gt)
		if item[0] == gt[0]:
		    wordcount += 1
		if item[1] == gt[1]:
		    poscount += 1
		if item[2] == gt[2]:
		    sercount += 1
	index += 1

	# Corrected segmented word and entity / total word count
	wordsegacc = wordcount/count
	posacc = poscount/count
	seracc = sercount/count

	print()
	print(t.info())
	print('Tagging time: ',time,' Accuracy: Word segmentation ',wordsegacc,' Pos tag ',posacc,' Entity recognition ',seracc)


	t.close()
	```
And there we have our desired metrics :
```
PyVi
Tagging time: 18.30862819995673 Accuracy: Word segmentation 0.5787813685605887 Pos tag 0.6820176441011108 Entity recognition 0.0

Underthesea
Tagging time: 38.3007470000739 Accuracy: Word segmentation 0.8004068199478904 Pos tag 0.6021163779311606 Entity recognition 0.0

VnCoreNLP
Tagging time: 67.42396240014205 Accuracy: Word segmentation 0.7836769209672259 Pos tag 0.6328564245554692 Entity recognition 0.0
```
We have tagging time along with accuracy for word segmentaion, pos tag and entity recognition (though entity missing from dataset so it always 0). 

Pyvi is the fastest of the lot, 2 times faster than the second fastest. This is the result of optimized SpaCy library. However, the trained model of PyVi lose out on segmentation accuracy, only managed 57.8%. Underthesea achived highest accuracy on segmentation at 80%, but lose out on pos tagging to both PyVi and VnCoreNLP. The java tool VnCoreNLP is the slowest of the lot due to it's java wrapper. I conclude that the best way to extract most correct token is mix match of using Underthesea for word segmentation, and PyVi for Pos tagging. 

We have no way to verify the correctness of data-set, but being part of the VLSP project, I assume it was rigorously verified.  Overall, on new data set, these tokenizers get no where near 95% segmentation accuracy they claimed. I hope the situation will improve in the future. One way forward is to have bigger training set for Vietnamese that these tools can benifit from. Recent developments in NLP using character segmentation makes Arabic language like Vietnamese instantly usable with any NLP tools is promissing. But most importantly, interest from more people from research community can push Vietnamese language progress go a long way.
