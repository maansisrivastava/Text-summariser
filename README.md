# tensorflow-text-summarization
Simple Tensorflow implementation of text summarization using [seq2seq library](https://www.tensorflow.org/api_guides/python/contrib.seq2seq).


## Model
Encoder-Decoder model with attention mechanism.

### Word Embedding
Used [Glove pre-trained vectors](https://nlp.stanford.edu/projects/glove/) to initialize word embedding.

### Encoder
Used LSTM cell with [stack_bidirectional_dynamic_rnn](https://www.tensorflow.org/api_docs/python/tf/contrib/rnn/stack_bidirectional_dynamic_rnn).

### Decoder
Used LSTM [BasicDecoder](https://www.tensorflow.org/api_docs/python/tf/contrib/seq2seq/BasicDecoder) for training, and [BeamSearchDecoder](https://www.tensorflow.org/api_docs/python/tf/contrib/seq2seq/BeamSearchDecoder) for inference.

### Attention Mechanism
Used [BahdanauAttention](https://www.tensorflow.org/api_docs/python/tf/contrib/seq2seq/BahdanauAttention) with weight normalization.


## Requirements
- Python 3
- Tensorflow (>=1.8.0)
- pip install -r requirements.txt


## Usage
### Prepare data
Dataset is available at [harvardnlp/sent-summary](https://github.com/harvardnlp/sent-summary). Locate the summary.tar.gz file in project root directory. Then,
```
$ python prep_data.py
```
To use Glove pre-trained embedding, download it via
```
$ python prep_data.py --glove
```

### Train
We used ```sumdata/train/train.article.txt``` and ```sumdata/train/train.title.txt``` for training data. To train the model, use
```
$ python train.py
```
To use Glove pre-trained vectors as initial embedding, use
```
$ python train.py --glove
```

### Test
Generate summary of each article in ```sumdata/train/valid.article.filter.txt``` by
```
$ python test.py
```
It will generate result summary file ```result.txt```. 

#### Sample Summary Output
```
"japanese share prices rose #.## percent thursday to <unk> highest closing high for more than five years as fresh gains on wall street fanned upbeat investor sentiment , dealers said ."
> Model output:  tokyo shares close # percent higher
> Actual title: tokyo shares close up # percent

"the final results of iraq 's december general elections are due within the next four days , a member of the iraqi electoral commission said on thursday ."
> Model output: iraqi election results due in next four days
> Actual title: iraqi election final results out within four days

"microsoft chairman bill gates late wednesday unveiled his vision of the digital lifestyle , outlining the latest version of his windows operating system to be launched later this year ."
> Model output: bill gates unveils new technology vision
> Actual title: gates unveils microsoft 's vision of digital lifestyle
```
