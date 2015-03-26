## What is Stylometry?

Stylometry is the application of the study of linguistic style, usually to written language, but it has successfully been applied to music and to fine-art paintings as well.

Stylometry is often used to attribute authorship to anonymous or disputed documents. It has legal as well as academic and literary applications, ranging from the question of the authorship of Shakespeare's works to forensic linguistics.

## Why write a library?

After searching through a wide variety of libraries for Python I was unable to find one that seemed to fit the needs. 

I also found a research paper that I really like that talked about applying machine learning techniques and stylometry. I decided to write a simple library while learning more about the NLTK python library. 

The initial version of the software took only about 3 hours and allowed me to extract a wide variety of features from text data. The library has since been extended into several different facets

## Data Preparation

1. Download Text from Gutenberg Project
2. Clean up Text files removing table of contents, licenses, etc
3. Split the file into 1000 line samples using ``split -l 1000 hamlet.txt -d`` to rename the output files consider using ``split --numeric-suffixes=1 --additional-suffix=.csv -l 1000 hamlet.txt hamlet_``
4. To rebuild the original data just ``cat book-0.txt book-1.txt book-2.txt > entire-book.txt``

I tried to make sure that every author have a similar number of lines and samples to analyze.

| Title | Author | Lines | Samples |
--- | --- | --- | ---
| Pride and Prejudice | Jane Austen | 13024 | 13 |
| Tale of Two Cities | Charles Dickens | 14496 | 14 |
| Romeo & Juliet Hamlet | William Shakespeare | 11077 | 11 |
| The Adventures of Huckleberry Finn | Mark Twain | 11433 | 10 |
| War and Peace - *Only Books 1 & 2* | Leo Tolstoy | 10517 | 11 |

## Demo

Download the repositories

```bash
$ git clone git@github.com:jpotts18/stylometry.git
$ git clone git@github.com:jpotts18/stylometry-data.git

$ ipython
```
Extract stylometry features from one document

```python
from stylometry import *
dickens1 = StyloDocument('stylometry-data/Dickens/tale-two-cities-0.txt')
dickens1.text_output()
```
Extract stylometry features from a set of documents called a corpus

```python
from stylometry import *

# Dickens Corpus
dickens_corpus = StyloCorpus.from_glob_pattern('stylometry-data/Dickens/*.txt')
dickens_corpus.output_csv('/Users/jpotts18/Desktop/dickens.csv')

# Shakespeare Corpus
shakespeare_corpus = StyloCorpus.from_glob_pattern('stylometry-data/Shakespeare/*.txt')
shakespeare_corpus.output_csv('/Users/jpotts18/Desktop/shakespeare.csv')

# Twain Corpus
twain_corpus = StyloCorpus.from_glob_pattern('stylometry-data/Twain/*.txt')
twain_corpus.output_csv('/Users/jpotts18/Desktop/twain.csv')

# Austen Corpus
austen_corpus = StyloCorpus.from_glob_pattern('stylometry-data/Austen/*.txt')
austen_corpus.output_csv('/Users/jpotts18/Desktop/austen.csv')

# Tolstoy Corpus
tolstoy_corpus = StyloCorpus.from_glob_pattern('stylometry-data/Tolstoy/*.txt')
tolstoy_corpus.output_csv('/Users/jpotts18/Desktop/tolstoy.csv')

# All authors
novel_corpus = StyloCorpus.from_glob_pattern('stylometry-data/*/*.txt')
novel_corpus.output_csv('/Users/jpotts18/Desktop/novels.csv')
```

Decision Tree Classificaiton

```python
from stylometry.classify import *
# splits data into validation and training default 80% train 20% validation
dtree = StyloDecisionTree('/Users/jpotts18/Desktop/novels.csv')
dtree.fit()
dtree.predict()
dtree.confusion_matrix()
dtree.write_tree()
```



