# Persian-Spelling-Correction
In this repository, I implemented two models using the Bert language model and Fasttext word embeddings. I used a pre-trained Bert model on the Persian language named Parstext. 

##### Dataset
the dataset was crawled from Persian news websites and was divided into four subjects: culture, economics, politics, and sports. In order to fine-tune our model, we used only 200,000 sentences of economics news.

##### Preprocessing
In this stage, we only normalized the sentences and used the tokenizer of the Bert model.

##### Model
We used the ParsBert, a Bert Model fine-tuned on the Persian language, to predict the masked word. 

The src folder contains two notebooks:
<ol>
<li>
Bert_and_MLP_for_MLM.ipynb

This notebook contains two approaches for spelling correction
<ul>
<li>
-the first approach

In this approach, we iterate over every single word in the sentence. In each iteration, we mask the word corresponding to that iteration in the sentence and give the masked sentence as input to the model. If the word was among the first predictions of the model, so the spelling is correct. Otherwise, we find the word among the predictions with the least edit distance to the misspelled word and give it back as the correct spelling.
</li>
<li>
-the second approach

In this approach, after applying masking to each word and not finding the word in the predictions, we compute the edit distance between each prediction and the misspelled word. Afterward, we choose the top five predictions with the minimum edit distance. We feed an MLP model with the edit distances and the ranking of the words in Bert's predictions(the words which are frequently used as replacements for the masked word will come first in the list of Bert's predictions). The MLP model identifies which word is the correction.

We trained our MLP model by creating variations of the words in the dataset that are spelled in the right way. The variations are simply created by adding or deleting a character to the word. Then the sentence with the misspelled word is fed to the Bert Model. We train the MLP model by feeding it with the Bert model prediction ranking and the edit distance. The loss is calculated by checking if the MLP found the word with true spelling or not.
</li>
</ul>
</li>
<li>
Transformer_fasttext.ipynb

In this notebook, same as the previous one, we detected the misspelled words using the Bert model. But, in order to find the true spelling, we chose the word having the embedding with the highest cosine similarity to the Fasttext embedding of the misspelled word.
</li>
</ol>
More explanations on the implementation are available in the notebooks.


