# Project dataset-sts: paper collection

## Basic concepts

Information Retrieval 
[wiki](https://en.wikipedia.org/wiki/Information_retrieval#R-Precision)

+ In the 1960s, the first large information retrieval research group was formed by Gerard Salton at Cornell.
+ Categorization of IR-models
<div align="center">
<img src="./img/05cate.png">
</div>
+ Performance and correctness measure
	+ Precision （除以检索文本） 
	+ Recall (除以相关的文本，即答案)
	+ Fall-out
	+ F-score/F-measure
	+ **Average precision**
	+ Precision at K
	+ R-precision
	+ **Mean Average precision** the mean of the average precision scores for each query
	+ Discounted cumulative gain
	+ **Mean reciprocal rank** - The mean reciprocal rank is the average of the reciprocal ranks of results for a sample of queries Q:  the rank position of the first relevant document for the i-th query.
	
## Content


### Model



## Results
### Dataset Size

<div align=center>
<img src = "./img/05dataset.png" width=80%>
</div>

### [Ubuntu-dialog](http://rover.ms.mff.cuni.cz/~pasky/ubuntu-dialog/)

+ Train set : 1M
+ Val set :  195600
+ Test set : 189200


| Model                    |Time / No.Model | valMRR   | val-2R@1 | val-10R@2 | testMRR  | test-2R@1 | test-10R@2 | settings
|--------------------------|----------|----------|----------|-----------|----------|-----------|------------|---------
| avg                      || 0.636035 | 0.805922 | 0.623185  | 0.639989 | 0.804449  | 0.626524   | ``inp_e_dropout=0`` ``dropout=0``
|                          ||±0.001673 |±0.001594 |±0.001892  |±0.001760 |±0.002325  |±0.002013   |
| DAN                     | 31s/epoch  No.54e02f6d97bdb55d|0.624075 | 0.793207 | 0.606736  | 0.628719 | 0.789763  | 0.611136   | ``inp_e_dropout=0`` ``dropout=0`` ``deep=2`` ``pact='relu'``
|                          ||±0.000384 |±0.000556 |±0.000761  |±0.000576 |±0.000664  |±0.000887   |
| rnn                      | 19043s/epoch  eta = 423 hr ||||||| ``sdim=1`` ``pdim=1`` ``ptscorer=B.dot_ptscorer``
| cnn                      | 2060747s/epoch eta = 45790 hr ||||||| ``pdim=1`` ``ptscorer=B.dot_ptscorer``
| rnncnn                   | 901966s/epoch eta = 20040hr||||||| ``sdim=1/2`` ``pdim=1`` ``ptscorer=B.dot_ptscorer``
| attn1511                 | 198683s/epoch eta = 4420 hr ||||||| ``sdim=1/2`` ``cdim=1/2`` ``ptscorer=B.dot_ptscorer``

These results are obtained like this:

	tools/train.py avg ubuntu data/anssel/ubuntu/v2-trainset.pickle data/anssel/ubuntu/v2-valset.pickle "vocabf='data/anssel/ubuntu/v2-vocab.pickle'" nb_runs=16 inp_e_dropout=0 dropout=0
	tools/eval.py avg ubuntu data/anssel/ubuntu/v2-trainset.pickle data/anssel/ubuntu/v2-valset.pickle data/anssel/ubuntu/v2-testset.pickle weights-ubuntu-avg--69489c8dc3b6ce11-*-bestval.h5 "vocabf='data/anssel/ubuntu/v2-vocab.pickle'" inp_e_dropout=0 dropout=0


### [yodaqa]()

+ Train set : 44648
+ Val set : 1149
+ Test set : 1518


| Model                    |Time  / No.Model    | trainAllMRR | devMRR   | testMAP  | testMRR  | settings
|--------------------------|-------------|-------------|----------|----------|----------|---------
| yodaqakw                 || 0.368773    | 0.337348 | 0.284100 | 0.383238 | (defaults)
| termfreq TF-IDF #w       || 0.339544    | 0.324693 | 0.242700 | 0.337893 | ``freq_mode='tf'``
| termfreq BM25 #w         || 0.483538    | 0.452647 | 0.294300 | 0.484530 | (defaults)
|--------------------------|-------------|-------------|----------|----------|----------|---------
| avg                      |~0s/epoch No.1ea55521db4b976d| 0.332093    | 0.429974 | 0.223231 | 0.311827 | (defaults)
|                          ||±0.004441    |±0.006614 |±0.002166 |±0.003241 |
| DAN                      |~0s/epoch No.18a6a3192e663b40|  0.400680    | 0.477409 | 0.251381 | 0.403253 | ``inp_e_dropout=0`` ``inp_w_dropout=1/3`` ``deep=2`` ``pact='relu'``
|                          || ±0.015806    |±0.011333 |±0.005232 |±0.016799 |
|--------------------------|-------------|-------------|----------|----------|----------|---------
| rnn                      |32s/epoch No.35579388742f9bd0 | 0.459869    | 0.429780 | 0.228869 | 0.341706 | (defaults)
|                          || ±0.035981    |±0.015609 |±0.005554 |±0.010643 |
| cnn                      |
| rnncnn                   ||                         
| attn1511                 ||--------------------------|-------------|----------|----------|----------|---------
| rnn                      |||--------------------------|-------------|-------------|----------|----------|----------|---------
| avg + BM25               || 0.523084    | 0.488757 | 0.258400 | 0.462038 |
|                          || ±0.016611    |±0.006357 |±0.003675 |±0.010558 |
| DAN + BM25               || 0.532890    | 0.505769 | 0.269900 | 0.486230 | ``inp_e_dropout=0`` ``inp_w_dropout=1/3`` ``deep=2`` ``pact='relu'``
|                          || ±0.022923    |±0.018970 |±0.006122 |±0.015990 |
| rnn + BM25               || 0.499628    | 0.486556 | 0.259022 | 0.466565 |
|                          || ±0.007046    |±0.007320 |±0.002541 |±0.005959 |
| cnn + BM25               || 0.588734    | 0.529302 | 0.271288 | 0.487668 |
|                          || ±0.028836    |±0.015761 |±0.003108 |±0.009528 |
| rnncnn + BM25            || 0.534710    | 0.478538 | 0.266200 | 0.473099 |
|                          || ±0.017699    |±0.010704 |±0.003659 |±0.011603 |
| attn1511 + BM25          || 0.486697    | 0.462924 | 0.271363 | 0.488915 |
|                          || ±0.012825    |±0.007982 |±0.001888 |±0.005102 |

These results are obtained like this:

	tools/train.py avg anssel data/anssel/yodaqa/curatedv2-training.csv data/anssel/yodaqa/curatedv2-val.csv nb_runs=16
	tools/eval.py avg anssel data/anssel/yodaqa/curatedv2-training.csv data/anssel/yodaqa/curatedv2-val.csv data/anssel/yodaqa/curatedv2-test.csv weights-anssel-avg--69489c8dc3b6ce11-*-bestval.h5
	

####large2470:

| Model                    |Time / No.Model | trainAllMRR | devMRR   | testMAP  | testMRR  | settings
|--------------------------|-------------|-------------|----------|----------|----------|---------
| yodaqakw                 || 0.332693    | 0.318246 | 0.303900 | 0.376465 | (defaults)
| termfreq TF-IDF #w       || 0.325390    | 0.328255 | 0.266800 | 0.362613 | ``freq_mode='tf'``
| termfreq BM25 #w         || 0.441573    | 0.432115 | 0.313900 | 0.490822 | (defaults)
|--------------------------|-------------|-------------|----------|----------|----------|---------
| avg                      || 0.798883    | 0.408034 | 0.262569 | 0.362190 | (defaults)
|                          || ±0.026554    |±0.004656 |±0.002054 |±0.005725 |
| DAN                      || 0.646481    | 0.404210 | 0.272675 | 0.386522 | ``inp_e_dropout=0`` ``inp_w_dropout=1/3`` ``deep=2`` ``pact='relu'``
|                          || ±0.070994    |±0.005378 |±0.003028 |±0.007627 |
|--------------------------|-------------|-------------|----------|----------|----------|---------
| rnn                      || 0.460984    | 0.382949 | 0.262463 | 0.381298 | (defaults)
|                          || ±0.023715    |±0.006451 |±0.002641 |±0.007643 |
| cnn                      || 0.550441    | 0.348247 | 0.264476 | 0.353243 | ``inp_e_dropout=1/2`` ``dropout=1/2`` (FIXME)
|                          || ±0.069701    |±0.006217 |±0.002918 |±0.009620 |
| rnncnn                   || 0.681908    | 0.408662 | 0.286118 | 0.394865 | (defaults)
|                          || ±0.114967    |±0.008659 |±0.003501 |±0.011895 |
| attn1511                 || 0.445635    | 0.408495 | 0.288100 | 0.430892 | (defaults)
|                          || ±0.056352    |±0.008744 |±0.005601 |±0.017858 |
|--------------------------|-------------|-------------|----------|----------|----------|---------
| Ubu. rnn                 || 0.623209    | 0.517763 | 0.359331 | 0.539284 | Ubuntu transfer learning (``ptscorer=B.dot_ptscorer`` ``pdim=1`` ``inp_e_dropout=0`` ``dropout=0`` ``balance_class=True`` ``adapt_ubuntu=True`` ``opt='rmsprop'``)
|                          |±0.014351    |±0.007724 |±0.003003 |±0.005755 |
|--------------------------|-------------|----------|----------|----------|---------
| avg + BM25               | 0.587060    | 0.461915 | 0.278288 | 0.480614 |
|                          |±0.031841    |±0.004170 |±0.002869 |±0.007834 |
| DAN + BM25               | 0.550746    | 0.466123 | 0.281512 | 0.490113 | ``inp_e_dropout=0`` ``inp_w_dropout=1/3`` ``deep=2`` ``pact='relu'``
|                          |±0.023915    |±0.003946 |±0.004459 |±0.010337 |
| rnn + BM25               | 0.496494    | 0.456415 | 0.276863 | 0.486928 |
|                          |±0.015167    |±0.007189 |±0.003576 |±0.008479 |
| cnn + BM25               | 0.616683    | 0.480827 | 0.287931 | 0.498852 |
|                          |±0.015484    |±0.004605 |±0.002607 |±0.006762 |
| rnncnn + BM25            | 0.569165    | 0.479459 | 0.287856 | 0.503145 |
|                          |±0.021074    |±0.004721 |±0.003550 |±0.009965 |
| attn1511 + BM25          | 0.531593    | 0.462170 | 0.286069 | 0.499340 |
|                          |±0.039245    |±0.006922 |±0.003301 |±0.008598 |
|--------------------------|-------------|----------|----------|----------|---------
| Ubu. rnn + BM25          | 0.540773    | 0.477836 | 0.290694 | 0.514866 | Ubuntu transfer learning (``ptscorer=B.mlp_ptscorer`` ``pdim=1`` ``inp_e_dropout=0`` ``dropout=0`` ``balance_class=True`` ``adapt_ubuntu=True`` ``opt='rmsprop'`` ``epoch_fract=1``)
|                          |±0.015786    |±0.005804 |±0.001523 |±0.003545 |
| SNLI rnn + BM25          | 0.601618    | 0.423854 | 0.264144 | 0.460083 | SNLI transfer learning (``dropout=0`` ``inp_e_dropout=0`` ``balance_class=True`` ``opt='rmsprop'`` ``epoch_fract=1``)
|                          |±0.092094    |±0.009707 |±0.005109 |±0.015438 |


These results are obtained like this:

	tools/train.py avg anssel data/anssel/yodaqa/large2470-training.csv data/anssel/yodaqa/large2470-val.csv nb_runs=16
	tools/eval.py avg anssel data/anssel/yodaqa/large2470-training.csv data/anssel/yodaqa/large2470-val.csv data/anssel/yodaqa/large2470-test.csv weights-anssel-avg--69489c8dc3b6ce11-*-bestval.h5

BM25 results are obtained with:

	"prescoring='termfreq'" "prescoring_weightsf='weights-anssel-termfreq-3368350fbcab42e4-bestval.h5'" "prescoring_input='bm25'" "f_add=['bm25']" prescoring_prune=20

Transfer learning has been performed like this:

	tools/transfer.py rnn ubuntu data/anssel/ubuntu/v2-vocab.pickle ubu-weights-rnn--23fa2eff7cda310d-bestval.h5 anssel data/anssel/yodaqa/curatedv2-training.csv data/anssel/yodaqa/curatedv2-val.csv pdim=1 ptscorer=B.dot_ptscorer dropout=0 inp_e_dropout=0 balance_class=True adapt_ubuntu=True "opt='rmsprop'" nb_runs=16
		tools/eval.py rnn anssel data/anssel/yodaqa/curatedv2-training.csv data/anssel/yodaqa/curatedv2-val.csv data/anssel/yodaqa/curatedv2-test.csv weights-ubuntu-anssel-rnn-1cd9ebbf9c99f926-*-bestval.h5 "vocabf='data/anssel/ubuntu/v2-vocab.pickle'" "vocabt='ubuntu'" pdim=1 ptscorer=B.dot_ptscorer inp_e_dropout=0 dropout=0 balance_class=True adapt_ubuntu=True "opt='rmsprop'"

where the ubu-weights model can be downloaded from

	http://pasky.or.cz/dev/brmson/ubu-weights-rnn--23fa2eff7cda310d-bestval.h5



### [Wang](https://code.google.com/p/jacana/)
The classic academic standard stems from the TREC-based dataset originally by Wang et al., 2007, in the form by Yao et al., 2013 

+ Train set : 44648
+ Val set : 1149
+ Test set : 1518


| Model                    |Time  / No.Model    | trainAllMRR | devMRR   | testMAP  | testMRR  | settings
|--------------------------|----------------|-------------|----------|----------|----------|---------
| termfreq TF-IDF #w       || 0.714169    | 0.725217 | 0.578200 | 0.708957 | ``freq_mode='tf'``
| termfreq BM25 #w         | |0.813992    | 0.829004 | 0.630100 | 0.765363 | (defaults)
| Tan (2015)               | |           |          | 0.728    | 0.832    | QA-LSTM/CNN+attention; state-of-art 2015
| dos Santos (2016)        | |            |          | 0.753    | 0.851    | Attentive Pooling CNN; state-of-art 2016
| Wang et al. (2016)       | |            |          | 0.771    | 0.845    | Lexical Decomposition and Composition; state-of-art 2016
|--------------------------|-------------|----------|----------|----------|---------
| avg                      |4s/epoch| 0.819778    | 0.869590 | 0.658770 | 0.751929 | (defaults)
|                          ||±0.005699    |±0.006494 |±0.007244 |±0.009579 |
| DAN.                     ||0.819778    | 0.869590 | 0.658770 | 0.751929 | ``inp_e_dropout=`` ``inp_w_dropout=1/3`` ``deep=2`` ``pact='relu'``
|                          ||±0.005699    |±0.006494 |±0.007244 |±0.009579 |
|--------------------------|-------------|----------|----------|----------|---------
| rnn                      |22s/epoch  No.5d241268d513bada| 0.838135    | 0.844555 | 0.693467 | 0.780089 | (defaults)
|                          ||±0.045370    |±0.008753 |±0.033851 |±0.028084 |
| cnn                      | 3s/epoch No.3166a0bfd4ea426e| 0.465275    | 0.630186 | 0.488169 | 0.566501 | (defaults)
|                          ||±0.085380    |±0.063493 |±0.045296 |±0.053307 |
| rnncnn                   | 17s/epoch No.48a6bd17e9a932f7| 0.539957    | 0.692048 | 0.552856 | 0.635640 | (defaults)
|                          ||±0.052861    |±0.030235 |±0.033240 |±0.034747 |
| attn1511                 | 17s/epoch No.353f0736c5245b72  | 0.879860    | 0.890741 | 0.710250 | 0.794669 | (defaults)
|                          ||±0.031631    |±0.066553 |±0.107366 |±0.162406 |
|--------------------------|-------------|----------|----------|----------|---------
| Ubu. rnn                 | ||||| Ubuntu transfer learning (``ptscorer=B.dot_ptscorer`` ``pdim=1`` ``inp_e_dropout=0`` ``dropout=0`` ``balance_class=True`` ``adapt_ubuntu=True`` ``opt='rmsprop'``)
|                          | |||||
|--------------------------|-------------|----------|----------|----------|---------
| avg + BM25               | ||||| 
|                          | |||||
| DAN + BM25               | ||||| ``inp_e_dropout=0`` ``inp_w_dropout=1/3`` ``deep=2`` ``pact='relu'``
|                          | |||||
| rnn + BM25               | |||||
|                          | |||||
| cnn + BM25               | |||||
|                          | |||||
| rnncnn + BM25            | |||||
|                          | |||||
| attn1511 + BM25          | |||||
|                          | |||||

These results are obtained like this:

	tools/train.py avg anssel data/anssel/wang/train-all.csv data/anssel/wang/dev.csv nb_runs=16
	tools/eval.py avg anssel data/anssel/wang/train-all.csv data/anssel/wang/dev.csv data/anssel/wang/test.csv weights-anssel-avg--69489c8dc3b6ce11-*-bestval.h5

BM25 results are obtained with:

	"prescoring='termfreq'" "prescoring_weightsf='weights-anssel-termfreq-3368350fbcab42e4-bestval.h5'" "prescoring_input='bm25'" "f_add=['bm25']" prescoring_prune=20


## Accumulate

* immanent 天生的，内在的 term-dependencies
* transcendent 超然的 term-interdependencies 
* fuzzy set 毛茸茸的，含混不清的
* wiggle 摇动
* interpolated 插入的
