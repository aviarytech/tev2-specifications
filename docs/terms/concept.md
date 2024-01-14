---
# Docusaurus header
id: concept
displayed_sidebar: tev2SideBar
# TEv2 Curated Text Header
term: concept
termType: concept
isa: semantic-unit
glossaryTerm:
glossaryText: "a [semantic unit](@) that captures the ideas/thoughts behind a classification of [entities](@) (what makes [entities](@) in that class 'the same')."
synonymOf:
grouptags:
formPhrases: [ "concept{ss}" ]
# Curation status
status: proposed
created: 20220606
updated: 20220606
# Origins/Acknowledgements
contributors: RieksJ
attribution: "[TNO Terminology Design](https://tno-terminology-design.github.io/tev2-specifications/docs)"
originalLicense: "[CC BY-SA 4.0](http://creativecommons.org/licenses/by-sa/4.0/?ref=chooser-v1)"
---

# Concept

### Summary
A **Concept** is a [semantic unit](@) that tries to capture the idea behind a classification of entities[^1], allowing us to reason about everything in the class as if it were one thing. For example, the ideas ([mental representations](https://en.wikipedia.org/wiki/Mental_representation)) you have when processing the sentences "I can drink beer from a beer glass' and 'I can drink beer from a coffee mug' shows that the concepts that are behind 'beer glass' and 'coffee mug' differ. Note that in order to communicate about this idea, we also need a word or phrase (i.e.: a [term](@)[^2] that we can use to refer to (instances of) this idea).

[^1]: WikiPedia has a concise [explanation of concepts](https://en.wikipedia.org/wiki/Concept). We use the term 'concept' as a [mental representation](https://en.wikipedia.org/wiki/Mental_representation).

[^2]: For the difference between 'Concept' and 'Term', see https://simple.wikipedia.org/wiki/Concept.

The [terminology pattern](pattern:terminology@) provides an overview of how this concept fits in with related concepts (as a specialization of a [semantic unit](@)).

### Purpose
Working together is easier when you and your peers share the same ideas. We need a way to test and ensure, that you and your peers _actually_ have the same understanding, for the purpose of making cooperation easier. Doing so is expected to not only reduce the number of terminological discussions, but also improve the efficiency and effectiveness of the remaining discussions.

### Criteria
A (description/specification of a) Concept MUST be [intensionally defined](https://en.wikipedia.org/wiki/Extensional_and_intensional_definitions), i.e. associated with a criterion that can be used to determine whether or not someone or something qualifies as (an instance of) that Concept, and that has the property that it has been shown that the vast majority of contributors and other users apply it in the same manner in different situations (i.e. they arrive at the same conclusion as to whether or not someone or something qualifies under that criterion in any given situation).

### Notes
There is an important [distinction between concepts and the (multitude of) terms](https://simple.wikipedia.org/wiki/Concept) (names, labels) that we need to be able to talk and reason (argue) about them. Please consider that

* different terms are used in different contexts for the same concept
* in different contexts, a single term may refer to different concepts
* to resolve terminological disputes, which usually are about the 'correct' meaning of a term, try to establish the criteria that the different participants use for the concept behind the term. That helps participants understand each others (different) positions, and provides a better basis for resolving the conflict.