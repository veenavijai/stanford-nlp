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


