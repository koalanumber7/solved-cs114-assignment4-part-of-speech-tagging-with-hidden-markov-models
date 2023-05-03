Download Link: https://assignmentchef.com/product/solved-cs114-assignment4-part-of-speech-tagging-with-hidden-markov-models
<br>
<h1></h1>

You are given pos tagger.py, and brown.zip, the Brown corpus (of partof-speech tagged sentences). Sentences are separated into a training set (≈80% of the data) and a development set (≈10% of the data). A testing set (≈10% of the data) has been held out and is not given to you. You are also given data small.zip, the toy corpus from HW2, in the same format. Each folder (train, dev, train small, test small) contains a number of documents, each of which contains a number of tokenized, tagged sentences in the following format: [&lt;word&gt;/&lt;tag&gt;]. For example:

The/at Fulton/np-tl County/nn-tl Grand/jj-tl Jury/nn-tl said/vbd Friday/nr an/at investigation/nn of/in Atlanta’s/np$ recent/jj primary/nn election/nn produced/vbd ‘‘/‘‘ no/at evidence/nn ’’/’’ that/cs any/dti irregularities/nns took/vbd place/nn ./.

<h2>Assignment</h2>

Your task is to implement a supervised hidden Markov model to perform part-of-speech tagging. Specifically, in pos tagger.py, you should fill in the following functions:

train(self, train set): This function should, given a folder of training documents, fill in self.initial, self.transition and self.emission, such that:

◦ self.initial[POS] = log(<em>P</em>(the initial tag is POS))

◦ self.transition[POS1][POS2] = log(<em>P</em>(the current tag is POS2|the previous tag is POS1))

◦ self.emission[POS][word] = log(<em>P</em>(the current word is word|the current tag is POS))

Some notes:

<ul>

 <li>The input sentence is a list. A list of what? You will need to translate from words to indices at some point in your code; it is recommended to do this translation outside of the viterbi function, so that the input to the function is a list of indices.</li>

 <li>Remember that when working in log-space, you should add the logs, rather than multiply the probabilities.</li>

 <li>Note that operations like + are Numpy <em>universal </em>functions, meaning that they automatically operate element-wise over arrays. This results in a substantial reduction in running time, compared with looping over each element of an array. As such, your viterbi implementation should not contain any for loops that range over states (for loops that range over time steps are fine).</li>

 <li>To avoid unnecessary for loops, you can use <em>broadcasting </em>to your advantage. Briefly, broadcasting allows you to operate over arrays with different shapes. For example, to add matrices of shapes (<em>a,</em>1) and (<em>a,b</em>), the single column of the first matrix is copied <em>b </em>times, to form a matrix of shape (<em>a,b</em>). Similarly, to add matrices of shapes (<em>a,b</em>) and (1<em>,b</em>), the single row of the second matrix is copied <em>a </em></li>

 <li>When performing integer array indexing, the result is an array of lower rank (number of dimensions). For example, if v is a matrix of shape (<em>a,b</em>), then v[:, t-1] is a vector of shape (<em>a,</em>). Broadcasting to a matrix of rank 2, however, results in a matrix of shape (1<em>,a</em>): our column becomes a row. To get a matrix of shape (<em>a,</em>1), you can either use slice indexing instead (i.e. v[:, t-1:t]), append a new axis of None after your integer index (i.e. v[:, t-1, None]), or use the reshape function (i.e. numpy.reshape(v[:, t-1], (a, 1))).</li>

 <li>In transition, each row represents a previous tag, while each column represents a current tag. Do not mix them up!</li>

 <li>Finally, you do not have to return the path probability, just the backtrace path.</li>

</ul>

3

test(self, dev set): This function should, given a folder of development (or testing) documents, return a dictionary of results such that:

◦ results[sentence id][‘correct’] = correct sequence of POS tags

◦ results[sentence id][‘predicted’] = predicted sequence of POS tags (hint: call the viterbi function)

You can assign each sentence a sentence id however you want. Your sequences of POS tags can just be lists, e.g. [‘at’, ‘np’, …].

evaluate(self, results): This function should return the overall accuracy (# of words correctly tagged / total # of words). You don’t have to calculate precision, recall, or F1 score.

<h2>Write-up</h2>

You should also prepare a (very) short write-up that includes at least the following:

How you set the value of <em>k</em>

Your evaluation results on the development set (you do not need to include any results on the toy data)