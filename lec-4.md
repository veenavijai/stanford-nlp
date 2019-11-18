 Lecture 4: Backpropagation

**Recap**

Storing a 'local' error, i.e., the common terms in the backpropagation, is useful to reduce computation. We basically reuse derivatives from higher layers to computer derivatives of lower layers.

'When we differentiate wrt one element of the matrix W(i, j), we get x(j). 

**Tips to Derive Gradients**

1. Define variables carefully and keep track of their dimension.
2. Be careful while using the chain rule.
3. While using softmax, take derivatives separately for the correct and incorrect class.
4. Take partial derivatives element-wise if it's confusing.
5. Stick to one shape convention.

**Deriving Gradients for Window Model**

In each update, we update one word in the window - hence the updates are sparse because most of the word vector is unchanged.

In principle, we hope that such gradient updates make the word vector more helpful in determining named entities.

Pitfall: Very similar words may not be moved around the same way while retraining

**Practically, should I use pre-trained word vectors?**

Yes, almost always, because they are trained on a huge amount of data and hence will know about words not in your dataset and even for the ones in your training set, they will know more. Unless you have a large corpus of labeled data (common in machine translation for popular languages), it is better to stick to pre-trained word vectors.

Regarding fine-tuning: For a small dataset (in the order of 100,000 words), it is typically better not ot update gradients at all. For a large dataset (> 1 million words), it probably works better to fine-tune the word vectors.

**Computation Graphs for Backprop**

Source nodes: inputs
Interior nodes: operations
Edges: pass along result of the operation

Forward propagation: is easy and just expression evaluation

Backpropagation: each node receives an upstream gradient and the goal is to pass on the correct downstream gradient. We use chain rule and the local gradient to help us with this:

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/69017412-bca7aa00-095b-11ea-9eec-e1b827d2bb08.png">
</p>

How do we deal with multiple inputs? We have multiple local gradients:

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/69017460-06909000-095c-11ea-9111-f32dadfe285b.png">
</p>

Note - multiple edges going into a node during backprop have their gradients summing up.

The computation graph helps us to calculate gradients efficiently.

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/69017637-30968200-095d-11ea-9ad2-5587047c4d48.png">
</p>

**Automatic Differentiation**

Gradient computation can be automatically inferred from the 'symbolic expression,' and the graph structure helps us calculate lower-layer derivatives via chain rule. 

Earlier frameworks like Theano did all the computation but modern frameworks like TensorFlow and PyTorch do backprop, but leave the local derivative calculation to you.

**Implementation Details**

Store all the values we need for backprop in the object class while doing forward propagation.

To check if your backprop implementation is correct, we can use numeric gradients (two-sided) to check. However, we can't actually use this because it is approximate and very slow. We would have to calculate the function f to be differentiated, for every parameter of the model. But this is useful to check implementation. 

**Regularization**

If we have a lot of parameters, they tend to memorize the training data - i.e., they overfit. We need to add a regularization term to avoid this.

**Vectorization**

Try to vectorize wherever possible and avoid loops, prefer vectors and matrices. Use timeit to check your improvement!

**Nonlinearities**

It is essential for deep learning networks to have some kind of non-linearities. Common non-linear functions are:
1. Logistic (sigmoid)
2. tanh 
3. Hard tanh (easier to compute but we don't want it to be -1 or +1)

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/69018159-d77c1d80-095f-11ea-8d68-7e05d2dba17e.png">
</p>

To avoid exponentials, people started using ReLU. They are very fast, train quickly, and perform well. Each unit is either dead or an identity function, after being passed through ReLU.

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/69018235-32ae1000-0960-11ea-8e99-1951952a90c9.png">
</p>

Variations are the parametric ReLU, where negative values are not suppressed to 0, but are assigned small negative values.

**Parameter Initializations**

Parameter weights should be initialized to small random values to avoid symmetries that prevent learning.

A common initialization is Xavier.

**Optimizers**

Usually, plain SGD works fine. However, we usually get better performance using a family of 'adaptive optimizers' which scale parameter adjustment with an accumulated gradient - such as Adagrad, RMSprop, Adam, SparseAdam, etc.

**Learning Rates**

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/69018427-4443e780-0961-11ea-876b-6d93f9b6d9f0.png">
</p>
