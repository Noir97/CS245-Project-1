# CS245-Project-1
Seed-based Weakly Supervised Named Entity Recognition



# Setup for Tensorflow-Named Entity Recognition (this is important wheather you want to do a train/test on model directly or do anyhting from start)

Note 

Utilizes tensorflow 1.13.1, but requires python 3.7.9 or lower. Can utilize pyenv to update local python enviornment.

Can either follow steps within the readme, but here's a summary
- Install tf_metrics
- Within the datafile (in our case testdata1) run make download-glove, will get the vector file
- An existing dataset exists within testdata1, you can change if wished, but if you just want to test the existing dataset, just directly run make build.
  - The current main function of the model utilizes a set format to train and test the files.
  - Training the model: select the tag file and rename it: train.tags.txt, and name the text file: train.words.txt
  - Testing files: same vein as training, but use testa.tags.txt / testa.words.txt and testb.tags.txt / testb.words.txt
- Current implementation only uses lstf_crf_ema model, but for more models check out https://github.com/guillaumegenthial/tf_ner
- Go to the models/lstf_crf_ema folder, and run python main.py (This will train the bi-lstm + crf on the dataset)
- This will generate a results folder that will include the score inside and have the predictions for each train/test dataset.
- To get recall,precision and f1 score, run ../conlleval < results/score/{name}.preds.txt > results/score/score.{name}.metrics.txt

- To generate a predicted tags file from an input, go to interact.py and change the INPUT_FILE. Updated to direclty read in a text file and return an output_tags.txt file.


<h1>If you want to run tagged training files directly and do the test</h1>
in tf_ner/data/testdata1, there are already files fit the input format of our model. Some of them are annotated using human annotation and some of them are based on entity set expansion. Here are the detail explainataions of each file:

ConLL:
<br/>
train_BERT_expand_tag_CoNLL.txt: the tagged training set on CoNLL using BERT
<br/>
train_Spacy_expand_tag_CoNLL.txt: the tagged training set on CoNLL using Spacy
<br/>
annotated_train_sentences_CoNLL.txt: The training sentences of CoNLL
<br/>
annotated_train_tags_CoNLL.txt: the original training tag of CoNLL.
<br/>

To train with the entity set expansion result, use train_BERT_expand_tag_CoNLL.txt OR train_Spacy_expand_tag_CoNLL.txt with annotated_train_sentences_CoNLL.txt. To train with oriignal sentence, use annotated_train_sentences_CoNLL.txt with annotated_train_tags_CoNLL.txt. 
<br/>
To test the training model on ConLL data, use annotated_test_sentences_CoNLL.txt with annotated_test_tags_CoNLL.txt as the test input. 
<br/>

A subsection of twitter data(section g):
<br/>
To train with the entity set expansion result, use train_twitter_g_sentences.txt with train_twitter_g_tags.txt. To train with oriignal sentence, use annotated_train_sentences_twitter_g.txt with annotated_train_tags.txt_twitter_g.txt
<br/>
To test the training model on test set of g, use annotated_test_tags.txt_twitter_g.txt and annotated_test_sentences_twitter_g.txt as the test input.
<br/>

Result leveraging the plain twitter data:
<br/>
To train the model on entity set expansion of plain twitter data only, please uncompress 0322_sentences_ and_tags.zip and get train_twitter_sentences_0322.txt and train_twitter_tags_0322.txt
<br/>
To train the model on entity set expansion of plain twitter data with the training data from broad twitter training set, please uncompress 0322_sentences_ and_tags.zip and get train_twitter_sentences_0322.txt and train_twitter_tags_0322.txt. THEN, concat the train_sentences.txt and train_twitter_sentences_0322.txt as a new training file, concat train_broad_tag_by_entity.txt with train_twitter_tags_0322.txt as the new tag file to train.
<br/>
To test the training model on test set, use test_tags.txt and test_sentences.txt as the test input.


<h1>If you want to do everything from the start</h1>
<b> Generated phrases from Twitter</b>


We put a example zip file (2020-03-22_clean-hydrated.zip) in the repository. This is the compressed result from one of our text file (2020-03-22_clean-hydrated.txt) as a example. We used Autophrase to mine the phrases from 7 text files like this. Due to the github size restrictions, we can't upload everything related to AutoPhrase to make it run. However, the process of running autoPhrase as follow:

<h5>NOTE</h5>
1. put your txt file into data/EN (in this case would be 2020-03-22_clean-hydrated.txt)
2. run auto_phrase.sh
3. find your result in model/DBLP, notice that the Autophrase.txt is the file we need(this file includes salient phrases).
4. Do a parsing to remove the scores in each line, and remove all phrases with 3 or more words.
<b>If you want to run AutoPhrase with full experience, please pull its original repo https://github.com/shangjingbo1226/AutoPhrase/tree/master/src and replace auto_phrase.sh with the one shown in our repository.</b>

We put the generated AutoPhrase.txt, rename those, in a folder called AutoPhrase result. This is the result doing step 1 to 3. We also include a parsing file to do step 4 in the same folder.

We put all the phrases generated by AutoPhrase mining the Twitter texts into the folder called phrase_list. Each file inside the folder is named after the date
of the original full twitter text file(i.e: 0322 stands for twitter recorded on 03/22/2020)

<b> CoNLL 2003 data</b>


We use CoNLL 2003 data as a small trial on using Spacy or BERT. The folder has train/test/validation split and the python code to parse the training/testing data
to the format accepted by Bi-LSTM-CRF is there. Using parsing_annotated_conll.py would retain the original tag of the input data and parse it to 2 files, one 
for sentence and one for tag. As these 2 files are required for Bi-LSTM model. parsing_conll_entity would do the tagging based on the expanded entity set. This
Should be only used for CoNLL training set to cover the original tags.

<b>Spacy expansion</b>

<b>Final Input generation</b>

<b> Evaluations and result</b> 
