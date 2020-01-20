# Dependency Parsing

**Linguistic Structure**

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/71702237-1be24200-2d83-11ea-80f8-76387988bf21.png">
</p>

We can club various constituents and give them new names. Foe example, a noun phrase consists of a determiner (a, the) followed by a noun. We can go on to define a more complex phrase of the form, "a large cat by the green crate" with the definitions below:

Noun phrase (NP) = determiner (+ adjective) + noun + prepositional phrase (PP)

PP = preposition + NP

Now we have the potential to generate infinitely big sentences using our definitions of phrase structures.

**Dependency Structure**

Which words modify other words?

<p align="center">
  <img width="550" height="150" src="https://user-images.githubusercontent.com/21968647/71702996-446c3b00-2d87-11ea-9a86-40ecaf373ccb.png">
</p>

Dependency structures help us understand language because humans are able to convey complex ideas by composing words to convey complex meanings. "We need to know what is connected to what."

Often, there are ambiguities in parsing, which could be avoided by figuring out the right relationship between words. A common ambiguity is the prepositional phrase attachment ambiguity, for instance: "scientists count whales from space" - are they counting the whales using a satellite? Or are the whales from space? 

Human languages, unlike programming languages, have ambiguity because it is assumed that the person on the other end can figure out the intended meaning. In a way, that makes human communication efficient because we don't have to explicity say everything to get the point across.

If we have consecutive PPs, the ambiguity can increase exponentially, with factorial terms involved.

'Coordination scope ambiguity' is another type of ambiguity:
<p align="center">
  <img width="550" height="150" src="https://user-images.githubusercontent.com/21968647/71703514-c8272700-2d89-11ea-8ebc-f5ce551fc68c.png">
</p>

Another example of the above is the headline on Trump's first physical exam, "Doctor: No heart, cognitive issues"

There are other types as well, such as Adjectival Modifier Ambiguity and Verb Phrase Attachment Ambiguity.

**Dependency Structures as Graphs**

We draw binary and asymmetric relationships, i.e., arrows, between 'lexical items' and represent it as a tree graph, where each arrow shows the relationship between parent (head of dependency) and child (dependent):

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/71703802-8bf4c600-2d8b-11ea-8baf-4f3a5dad970f.png">
</p>

Panini, a famous linguist, was creating dependency structures for Sanskrit as early as 5 BC!

### Treebanks and their Advantages

Universal Dependencies is a database with manually annotated 'treebanks.'

Writing grammars instead of dependencies seems more efficient because they can probably generalize well and take less time. However, treebanks are quite useful because they are reuse-able to build parsers, whereas grammars vary a lot depending on who came up with the definition. Another huge advantage is that treebanks can help find the correct structure for the sentence in context, which grammars cannot do - this helps a lot when it comes to ambiguity.

### Constraints with Dependency Structures

1. We want our graph to be a tree, so no cycles are allowed.
2. Often we have a 'fake' node called ROOT, which links to the very first dependency head - there should be only one such word which is connected to ROOT.

### Arc-standard Transition-based Parser

We have a stack (words we;ve seen) and a buffer (words which are unseen). At every stage, we can take a word from the buffer, and shift it onto the stack, or we can look at the existing stack and either create a 'left arc' or a 'right arc' to represent a dependency relationship.

However, we have many choices at each stage - how do we know the correct way to make choices? If we looked at all choices, we would have to consider an exponential number of possible trees. Instead, people in the 1960s used dynamic programming to efficiently go through the search space. They were still cubic in their order, or worse.

Later in the 2000s, Joakim Nivre came up with the idea to use machine learning - each of the 3 possible actions (shift, left arc, right arc) is predicted by a discriminative classifier, which does quite well. It provides linear time parsing, which was a huge breakthrough.


### Conventional Feature Representation

In order to build the classifier, we had very complex hand-engineered features (which were binary indicator features).

Note: we can generate a parse and count the number of correct dependencies in order to evaluate a parser.

There are problems with complex hand-engineered features:

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/72720622-395b2e00-3b2f-11ea-9d4b-313ca0bc0ee9.png">
</p>

To avoid the problems that arise with conventional parsers, we could build a neural network to predict an action given a particular stack+buffer configuration - this is neural dependency parsing. Chen & Manning achieved SOTA with even faster speeds in 2014.

The idea was to use the input features as distributed representations (exactly like word vectors) and have the network predict the action (similar to the actions in Nivre's model).

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/72721172-49bfd880-3b30-11ea-89c9-062ba66d8ba5.png">
</p>

The latest work is similar to this, using deeper, more finely tuned networks, along with beam search (done by Google).
