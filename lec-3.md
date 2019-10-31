# Lecture 3: Neural Networks

**Notation**

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/64905019-261eea00-d687-11e9-96ae-0c55b4c38da0.png">
</p>

Aim of a classifier: Given data x<sub>i</sub>'s of 2 classes, we want to 'learn a line' which separates them. This line is our classifier. We multiple the x<sub>i</sub>'s by an estimated weight vector, which then goes into our classifier decision. Here's a softmax classifier:

<p align="center">
  <img width="650" height="200" src="https://user-images.githubusercontent.com/21968647/64905190-4c458980-d689-11e9-8f9a-04485f6e8816.png">
</p>

The softmax classifier assigns P (y | x) as the softmax function with the numerator being the dot product of the y<sup>th</sup> row of W and x, and the denominator as the sum of that dot product for every row of W with x.

Note - Logistic regression classifiers and softmax classifiers are linear, so the decision boundaries are linear in some suitably high dimension.

Note - for logistic regression, no. of weight vectors we need = no. of classes - 1. For softmax, we will need c vectors.

**Loss Functions**

For each example (x, y), we want to maximize P(y | x) for the correct y. This is equivalent to minimizing - log P( x | y), which is negative log likelihood. Deep learning frameworks have this 'NLL loss.'

However, we more commonly use 'cross-entropy loss,' which is a measure of how good our estimated probability distribution is. Cross-entropy is defined as:

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/64905190-4c458980-d689-11e9-8f9a-04485f6e8816.png">
</p>

When we have one-hot encoding, the cross-entropy loss becomes the NLL. However, for a given instance, we may not have that one-hot encoding and instead have a probability for each class. Therefore, cross-entropy is a more general loss function.
