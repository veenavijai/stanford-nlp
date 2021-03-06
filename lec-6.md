# Language Models and RNNs

**Language Model**

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/72767264-6566c580-3ba8-11ea-95dd-dbe6289bfed6.png">
</p>

We can therefore view a language model as performing a classification task, because there is a predefined number of possibilities for the word to be predicted.

**n-gram Models**

<p align="center">
  <img width="550" height="200" src="https://user-images.githubusercontent.com/21968647/72767598-64826380-3ba9-11ea-8a49-3e4ccea3985e.png">
</p>

We make the simplifying assumption that the probability of the word only depends on the probability of the previous (n - 1) words. We applying the definition of conditional probability to say that 

P (x<sub>t+1</sub> | x<sub>t</sub>, x<sub>t-1</sub>, ... , x<sub>t-n+2</sub>) = P (x<sub>t+1</sub>, x<sub>t</sub>, x<sub>t-1</sub>, ... , x<sub>t-n+2</sub>) / P (x<sub>t</sub>, x<sub>t-1</sub>, ... , x<sub>t-n+2</sub>)

We obtain the above probabilities by actually counting them in a large corpus of text.

Example:

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/72769716-76b3d000-3bb0-11ea-9233-d1e82d6f2c4d.png">
</p>

**Problems with n-gram Models**

1. By throwing away context, we might actually be doing worse.
2. Sparsity - if the count of a particular word is 0 in the given corpus, it will never be predicted. We can deal with this by adding a small number delta to the numerator and denominator - this is called "smoothing" because it helps reduce sharp spikes.
3. What if the denominator is 0? i.e., the phrase, "the students opened their" itself never occurs. In this case, we would "backoff" to a lesser value of n for the n-gram model, and maybe only consider "opened their," in order to at least calculate some probability distribution.

#1 and #3 are tradeoffs against each other, because if we want more context, we would choose a largervalue of n, which would make our distribution more sparse. Typically, we don't use n > 5.

**n-gram Models for Language Generation**

We can find the conditional probabilities for the next word and rank them in descending order. The next word can be chosen by randomly sampling the top 'k' results. And the process continues.

**Neural Language Models**

We concatenate the word embeddings of the previous (n - 1) words and pass it through some hidden layers and finally to a softmax layer, which outputs the probability of each word.

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/72770389-96e48e80-3bb2-11ea-844c-c6c7d33ed052.png">
</p>

We have a few improvements over n-gram but also disadvantage. The biggest disadvantage is that the weight matrix, W, has no weights which are shared across embeddings - they should have some commonalities but we aren't making use of them, and we are probably causing redundancies.

<p align="center">
  <img width="350" height="550" src="https://user-images.githubusercontent.com/21968647/72770562-16725d80-3bb3-11ea-9d6b-1625d26807b5.png">
</p>

**Recurrent Neural Network (RNN) Architecture**

Each hidden state is calcuated using the previous hidden state and the current input. The core idea in an RNN is that the weights are shared in every stage. Since we don't apply different weights at each step, we are able to handle variable-length input.

<p align="center">
  <img width="550" height="250" src="https://user-images.githubusercontent.com/21968647/72770722-8e408800-3bb3-11ea-882f-704bf55b9837.png">
</p>

Our hidden states are calculated as follows:

<p align="center">
  <img width="300" height="90" src="https://user-images.githubusercontent.com/21968647/72770789-c0ea8080-3bb3-11ea-841f-805d18035f28.png">
</p>

The embeddings used can be initialized and trained from scratch, or pre-trained embeddings which are 'frozen' and left unchanged, or fine-tuned pre-trained embeddings.

<p align="center">
  <img width="200" height="400" src="https://user-images.githubusercontent.com/21968647/72771469-d496e680-3bb5-11ea-90d2-84e60609df3f.png">
</p>

**Training an RNN Language Model**

At every time step, we predict the probability distribution of every word (to be the next word), and we compute loss as cross-entropy between our predicted distribution and the actual next word in the corpus (which will be a one-hot vector).

<p align="center">
  <img width="400" height="250" src="https://user-images.githubusercontent.com/21968647/73156703-76757200-4093-11ea-92bd-a3c347f7347a.png">
</p>

However, updating gradients after an entire pass across the corpus takes time. So, in practice, we usually use Stochastic Gradient Descent. Remember: SGD on average gives the same descent direction as GD, and also it is a good idea to shuffle data.

**Backpropagation**

We use the multivariable chain rule to write the derivative of the loss function wrt the weight matrix as the sum of the derivatives at each time step. This accumulation is referred to as 'backpropagation through time.

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/73157474-ed136f00-4095-11ea-8667-a86c88ee58b9.png">
</p>

**Generating Text with RNN Models**

Similar to n-gram models, we repeatedly sample new words from the predicted probability distribution and use it as the next input.

Be skeptical while looking at generated text online, as they are usually cherry-picked and may be edited. 

**Evaluating Language Models**

A standard metric is perplexity, which is actually the e<sup>cross-entropy loss</sup>. This is convenient because reducing cross-entropy loss will also reduce the perplexity. We want the lowest possible perplexity.

<p align="center">
  <img width="450" height="250" src="https://user-images.githubusercontent.com/21968647/73157474-ed136f00-4095-11ea-8667-a86c88ee58b9.png">
</p>

The 1/T in the exponent is to ensure that the perplexity doesn't get larger just because of the size of the corpus.

Langauge Modelling is important because it is a task which provides a benchmark for how well we understand language. It is also a subcomponent of many other NLP components such as predictive typing, speech recognition, handwriting recognition, spelling and grammar correction, machine translation, author identification, summarization, dialogue systems, etc.

Note: RNNs are not Language Models by themselves; rather, they are a way to build an LM. RNNs are also good for part-of-speech tagging, named entity recognition, sentiment classification, etc. In question-answering applications, the RNN acts as an encoder. In cases where we have inputs such as speech/audio/music/images, we would use a conditional language model.
