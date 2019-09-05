# Lecture 2: Word Vectors and Word Senses

In addition to the vector space being a 'similarity space,' where words which are close together have similar meanings, we may also be able to associate different directions with different meanings. The most famous example of this is the analogy "Man:king :: Woman:queen." It seems to understand notions of similarity (Australia-Australian), of belonging (France-champagne), syntax (long-longest), of the notion of making something extreme (bad-terrible), and more.

Note - take PCA 2-D representations with a pinch of salt, because a lot of information is lost while bringing it down to 2 dimensions.

**Review of Word2Vec**

We have a matrix for U and V, which are the matrices of vectors for when the word is an 'outside-word' and 'centre-word' respectively. Note - in most major deep learning packages like TensorFlow and PyTorch, a vector is represented as a row, not as a column. The dot product we take is U.vT which is n x 1 if we have n words. The softmax vector is also n x 1 which gives us the probabilities.

The aim of Word2Vec is to have a model that outputs reasonably high probabilities for all words occurring in the context of a given 'centre-word.'

However, because of the above, we always get high probability estimates for words like 'that,' 'and,' 'the,' etc. A crude way of fixing this issue is to just remove those first few high-frequency estimates in the word vector.

Again, we need to remember that 2D plots can be very misleading, because we can't have a word close to all the words it is similar to, in different contexts. For example, Nokia can be close to Samsung, but also to Finland, which is not possible in 2D. In higher dimensions, it is possible for a given word to be close to multiple words in different directions.

**Detour: Optimization**

We have a cost function to minimize. We calculate the gradient of the cost function. We take small steps in the direction of the gradient (move downhill) to reach a minimum. 'Small steps' are achieved through a small learning rate, alpha, which is multiplied with the calculated gradient and then subtracted from the old parameter.







On a computer, we can use a thesaurus like say, WordNet, which gives us different synonym lists (for different contexts), and also hypernyms (relationships).
<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/21968647/63219238-4740f900-c122-11e9-9178-cf47034a6d93.png">
</p>


Problems with one-hot encoding:
1. Vectors end up having very high dimension.
2. There is no notion of similarity. Eg: 'motel' and 'hotel' are very similar, but their one-hot encodings are orthogonal. How do we get past this?    
    i. We could build a similarity matrix - but that would have huge size.
    ii. We could figure out a way to represent words which could directly give the similarity of that word with another word - 'distributional semantics.'



