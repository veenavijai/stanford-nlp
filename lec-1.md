# Lecture 1

At first glance, human language might seem inefficient compared to increasing bandwidth. However, we can think of it as a way to compress a lot of information. For example, I may speak one sentence which has enough information for you to visualize a whole scene.

A common linguistic way of thinking about meaning is denotational semantics. The 'meaning' is what the word 'represents.'

**WordNet**

On a computer, we can use a thesaurus like say, WordNet, which gives us different synonym lists (for different contexts), and also hypernyms (relationships).
<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/21968647/63219238-4740f900-c122-11e9-9178-cf47034a6d93.png">
</p>

Problems with WordNet: 
1. Misses nuances - for example, proficient doesn't always mean good
2. Requires humans to manually update it to add new words
3. Has very fixed synonym sets - so even if two words are partially similar, WordNet will not recognize it, like 'good' and 'marvellous'

**Representation of Words**

In traditional NLP, words are regarded as discrete symbols - this is a 'localist' approach.

Words can be represented as one-hot vectors, where we have 1 to represent that word, and 0 for every other word in the dictionary. 

Problems with one-hot encoding:
1. Vectors end up having very high dimension.
2. There is no notion of similarity. Eg: 'motel' and 'hotel' are very similar, but their one-hot encodings are orthogonal. How do we get past this?    
    i. We could build a similarity matrix - but that would have huge size.
    ii. We could figure out a way to represent words which could directly give the similarity of that word with another word - 'distributional semantics.'

**Distributional Semantics**

"A word's meaning is given by the words that appear frequently close by."

Idea: We look at different sentences containing a word w, and understand w based on the contexts in which we see it occurring. More specifically, the 'context' of w is the set of words which appear nearby - within a limited window.

Now, the word w is has a 'distributed representation' for which we use a 'dense' vector - this has all non-zero entries.

We have a 'vector space' in which we represent all words. This can be visualized in 2D though the size of the word representation may be 50, 300, 1000, 2000, 4000, etc.

Note - The basis vectors aren't fixed, because we could change the basis vectors keeping the dimension the name, and we wpild still be using the same vector space. Later we will come across learning algorithms where we input a bunch of text and get the word representation (with a particular dimension) as the output.

**Word2Vec: Tomas Mikolov**

A simple and scalable way of learning vector representations of words.

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/21968647/63219490-cd137300-c127-11e9-8089-48d76d50c103.png">
</p>

The vectors are initialized randomly. Then, we go through an iterative process where we go through each position in the text, and look at the surrounding words 

Graphically understanding the 'centre word' c and the 'outside-context words' o:

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/21968647/63219503-fcc27b00-c127-11e9-8b9f-3bc61c64b815.png">
</p>

Our aim is to maximize the likelihood: for a given centre word c, we want to maximize P ( o | c) for all 'outside' words within a certain window, and repeat this process for every 'centre-word.'

We set the objective function as negative log likelihood. Upon minimizing, we get the optimal value of theta, which represents the best vector representations of the word.

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/21968647/63641936-9da2c000-c66b-11e9-82f7-1c68863146f0.png">
</p>

Our parameter is theta - but instead of theta representing the vector representation of a word, we will use *two* vector representations per word: one, when it is the centre word, and two when it is an outside word.

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/21968647/63642028-09d1f380-c66d-11e9-9f5b-2747bd29d404.png">
</p>

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/21968647/63642203-d1ccaf80-c670-11e9-9754-a8f5ab92a437.png">
</p>

In this way, we calculate P (o | c) as the softmax of the "outside-vector" vector of o and the "centre-vector" vector of c. Note - the denominator is to normalize over the entire volcabulary, and not just the window around c. The words o, which are closer to c in the vector space, have higher dot product, and hence, higher P(o | c).
Note - The softmax mainly puts mass where the maximum is.

Now we find the value of theta that maximizes the likelihood and minimizes the objective function using gradient descent. In every iteration of gradient descent, we update the vector representations in theta to get higher probabilites P(o | c).

**Gradient Calculation and Interpretation**

<p align="center">
  <img src="https://user-images.githubusercontent.com/21968647/63642994-0cd4e000-c67d-11e9-8915-7949099857c3.png">
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/21968647/63643006-3a218e00-c67d-11e9-9f7f-ad87c6295f70.png">
</p>

Takeaway: We can interpret the calculated gradient as "observed representation of teh context word" - "expected representation of the context word."








