# Lecture 3: Neural Networks

**Notation**

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/64905019-261eea00-d687-11e9-96ae-0c55b4c38da0.png">
</p>

Aim of a classifier: Given data x<sub>i</sub>'s of 2 classes, we want to 'learn a line' which separates them. This line is our classifier. We multiple the x<sub>i</sub>'s by an estimated weight vector, which then goes into our classifier decision. Here's a softmax classifier:

<p align="center">
  <img width="750" height="150" src="https://user-images.githubusercontent.com/21968647/64905190-4c458980-d689-11e9-8f9a-04485f6e8816.png">
</p>

The softmax classifier assigns P (y | x) as the softmax function with the numerator being the dot product of the y<sup>th</sup> row of W and x, and the denominator as the sum of that dot product for every row of W with x. Essentially, the softmax function returns a probability distribution corresponding to Wx.

Note - Logistic regression classifiers and softmax classifiers are linear, so the decision boundaries are linear in some suitably high dimension.

Note - for logistic regression, no. of weight vectors we need = no. of classes - 1. For softmax, we will need c vectors.

**Loss Functions**

For each example (x, y), we want to maximize P(y | x) for the correct y. This is equivalent to minimizing - log P( x | y), which is negative log likelihood (NLL). Deep learning frameworks often use the NLL loss.

However, we more commonly use 'cross-entropy loss,' which is a measure of how good our estimated probability distribution is and is more convenient to use than NLL. Cross-entropy is defined as:

<p align="center">
  <img width="500" height="150" src="https://user-images.githubusercontent.com/21968647/68459919-7617ba00-01bb-11ea-9379-6d96028b7a00.png">
</p>

p is the true probability distribution, and q is the probability distribution as given by softmax. While training, we usually consider the objective function to be the mean of the cross-entropy loss.

When we have one-hot encoding, the cross-entropy loss becomes the NLL, because only one of the p terms is non-zero. However, for a given instance, we may not have that one-hot encoding and instead have a probability for each class. Therefore, cross-entropy is a more general loss function.

**Traditional ML Optimization**

Our parameter theta is vectorized version of W, because we are trying to learn the weights. We get the gradient of our objective function and use an algorithm (like, say, gradient descent) to minimize the loss function.

**Neural Network Classifiers**

Logistic regression only gives linear boundaries (high bias). This is not very powerful and in fact, is quite limiting, when the problem is complex.

For natural signals like speech and images, it would be too simplistic to fit a linear classifier. So, we use neural networks to learn more complex and non-linear decision boundaries.

**Classification with Word Vectors**

Refresher: we are trying to represent a word as a vector of real numbers and capture a representation which is useful to us. This is a subfield of deep learning called 'representation learning.'

Up to now, the objective function contained the vectorized version of W. Now, we also add the word vectors for each word. Therefore, we are simultaneously optimization the weights W and the word representations.Theoretically, this makes sense, but if the word vectors are one-hot vectors, the dimension of J is huge, so no one actually uses such an objective function.

**Artificial Neurons**

We weight the inputs and use an activation function, which assigns outputs based on a simple threshold.

We can obtain a vector of outputs too, as opposed to a single output. We don't actually have to hand-design which variables we want the neural network to learn. 

Deep learning: multiple layers with multiple neurons can probably learn sophisticated functions efficiently.

We calculate the 'activation' from the output of a layer, and we do this element-wise, to make the whole process non-linear - otherwise, multiple layers would just represent one linear transformation, which can be representation by one matrix. So, the classifier wouldn't have any extra power.

**Named Entity Recognition (NER)**











