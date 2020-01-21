# Language Models and RNNs

**Language Model**

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/72767264-6566c580-3ba8-11ea-95dd-dbe6289bfed6.png">
</p>

We can therefore view a language model as performing a classification task, because there is a predefined number of possibilities for the word to be predicted.

**n-gram Models**

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/72767598-64826380-3ba9-11ea-8a49-3e4ccea3985e.png">
</p>

We make the simplifying assumption that the probability of the word only depends on the probability of the previous (n - 1) words. We applying the definition of conditional probability to say that 

P (x<sub>t+1</sub> | x<sub>t</sub>, x<sub>t-1</sub>, ... , <sub>t-n+2</sub>) = P (x<sub>t+1</sub>, x<sub>t</sub>, x<sub>t-1</sub>, ... , <sub>t-n+2</sub>) / P (x<sub>t</sub>, x<sub>t-1</sub>, ... , <sub>t-n+2</sub>)
