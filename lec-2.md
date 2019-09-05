# Lecture 2: Word Vectors and Word Senses

In addition to the vector space being a 'similarity space,' where words which are close together have similar meanings, we may also be able to associate different directions with different meanings. The most famous example of this is the analogy "Man:king :: Woman:queen." It seems to understand notions of similarity (Australia-Australian), of belonging (France-champagne), syntax (long-longest), of the notion of making something extreme (bad-terrible), and more.

Note - take PCA 2D representations with a pinch of salt, because a lot of information is lost while bringing it down to 2 dimensions.

**Review of Word2Vec**

We have a matrix for U and V, which are the matrices of vectors for when the word is an 'outside-word' and 'centre-word' respectively. Note - in most major deep learning packages like TensorFlow and PyTorch, a vector is represented as a row, not as a column. The dot product we take is U.vT which is n x 1 if we have n words. The softmax vector is also n x 1 which gives us the probabilities.

The aim of Word2Vec is to have a model that outputs reasonably high probabilities for all words occurring in the context of a given 'centre-word.'

However, because of the above, we always get high probability estimates for words like 'that,' 'and,' 'the,' etc. A crude way of fixing this issue is to just remove those first few high-frequency estimates in the word vector.

Again, we need to remember that 2D plots can be very misleading, because we can't have a word close to all the words it is similar to, in different contexts. For example, Nokia can be close to Samsung, but also to Finland, which is not possible in 2D. In higher dimensions, it is possible for a given word to be close to multiple words in different directions.

**Detour: Optimization**

We have a cost function to minimize. We calculate the gradient of the cost function. We take small steps in the direction of the gradient (move downhill) to reach a minimum. 'Small steps' are achieved through a small learning rate, alpha, which is multiplied with the calculated gradient and then subtracted from the old parameter. Note - our objective function may not always be convex!

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/64314668-a5643d80-cf64-11e9-99e5-34c21a6235fc.png">
</p>

The problem with gradient descent is that if we take, say 1 billion words, and consider 10 context words for each centre word, we will have to calculate 10 billion gradients before we can make a single update, which is extremely slow. This is not done practically.

Rather, we can use Stochastic Gradient Descent (SGD). We repeatedly sample windows and make an update after each one. It is a noisy estimate of the gradient, but the estimate improves as we take more and more centre words. Normally, we sample a small bunch, or a 'minibatch' of say, 32 or 64 words, and calculate the gradient update from each minibatch. Also, to use parallel programming with GPUs, it is better to use powers of 2 for batch sizes.

If we do use SGD, then only some words occur in its context - not every word vector is updated. This makes the update vector very sparse. We only need to update the word vectors of words which actually appear, so either we need sparse matrix udpate operations to update certain rows of U and V, or we can maintain a hash for word vectors.

**Why do we use 2 vectors in Word2Vec?**

If we have two vectors for each word - for when it is a centre-word and one when it is an outside-word, gradient computation is straightforward. Otherwise, if we use only one word vector - consider a centre-word w. We will have to consider w itself while calculating the softmax of its outside-words, which leads to square terms. Having 2 separate word vectors actually simplifies the math.

**Family of Word2Vec models**

1. Skip-grams (SG)
    i. One centre-word
    ii. We try to predict all the context-words, one at a time
2. Continuous Bag of Words (CBOW)
    i. We have all the context-words
    ii. We try to use all of them, by considering them independently like a Naive Bayes model, to predict the centre word
    
**Negative sampling to improve efficiency in training**

Calculating the denominator, which is a sum of dot products computed for *every* word in the vocabulary, is very computationally expensive.

<p align="center">
  <img width="700" height="200" src="https://user-images.githubusercontent.com/21968647/64316195-f70ec700-cf68-11e9-802f-3e65f2cc208f.png">
</p>

So for a standard Word2Vec, the skip-gram model is implemented with negative sampling.

Idea of negative sampling: Train binary logistic regressions for 
1. A true pair, which is w & c (a word + one if its context-words) - we try to assign a high probability to c, because it is a word that was actually observed
2. Several noise pairs, which is w & c' (a word + some of its non-context-words) - randomly sample some non-context words, and assign a low probability to them

<p align="center">
  <img width="600" height="300" src="https://user-images.githubusercontent.com/21968647/64381767-cb332600-cfe8-11e9-8b58-d5481265d691.png">
</p>

For the good words (observed context words), we take the dot product with the centre word, put it through a sigmoid function, and we want this probability estimate to be as high as possible.

On the other hand, we also have K randomly chosen words, and we want the sigmoid of their dot product to be as small as possible - so we have an extra minus sign there. We usually take around 10-15 negative samples. 

**Unigram distribution**

To deal with the problem of very frequent words always appearing as context words and subsequently having high probability always assigned to them, we use a 'unigram' distribution to sample words. 

We obtain counts for the occurrence of each word in the corpus - these are 'unigram counts.' These counts are raised to 3/4  (a hyperparameter), and it has the effect of decreasing how often we sample common words and increasing how often we sample rarer words. 

**Co-occurrence Matrix**

Why don't we just count all the words occuring near a given word w, within a particular window, and construct all our probabilities from this co-occurrence matrix?

This co-occurrence matrix will be symmetric and probably sparse, if the window size is small.

Problems with this method:
1. We need O(n<sup>2</sup>) storage, which is a lot of storage especially if the vocabulary is large.
2. Classification models might have sparsity issues, and hence may not be very robust.
    
Solution: We can represent this co-occurrence matrix using a low-dimensional matrix that preserves all the information and is dense. Its dimension would be 25-1000, similar to Word2Vec.

**Dimensionality Reduction**

We factorize the matrix X into USVT and then throw away the smaller singular vectors to get the 'reduced SVD.' Now we have low-dimension SVD word vectors, which we can plot, like before. Note - When we use k singular values, that approximation is actually the *best* k-rank approximation of the matrix in a least-squares error sense. 

This technique was used in applications like 'Latent Semantic Analysis' and 'Latent Semantic Indexing' and the idea was that we would have semantic directions with some meaning, that we were finding in low-dimensional spaces.

**Other work-arounds for frequently occurring words**

1. Scale the counts using, say, log - commonly used in information retrieval
2. Use a ceiling function - min (X, t) with t = 100, and then ignore counts with 100
3. Sample context-words closer to the centre-words, more - 'ramped windows'
4. Use Pearson correlations instead of counts and discard negative values

**Semantic patterns in vector space**

If we can construct a vector space with a linear property, i.e., clearly associate a direction to a relationship (for example, action-doer like clean-janitor, swim-swimmer, pray-priest), then these word vectors will do well when it comes to analogies.

Takeaway: if we carefully design the counts, even conventional methods can give us useful word vector spaces.


**Count-based vs direct prediction**


    

    








