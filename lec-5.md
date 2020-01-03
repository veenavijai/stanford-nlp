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

**Dependency Structures**

We draw binary and asymmetric relationships, i.e., arrows, between 'lexical items' and represent it as a tree graph:

<p align="center">
  <img width="550" height="350" src="https://user-images.githubusercontent.com/21968647/71703802-8bf4c600-2d8b-11ea-8baf-4f3a5dad970f.png">
</p>



