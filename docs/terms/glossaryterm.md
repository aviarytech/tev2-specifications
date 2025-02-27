---
# Docusaurus header
id: glossaryterm
displayed_sidebar: tev2SideBar
# TEv2 Curated Text Header
term: glossaryterm
termType: concept
isa: 
glossaryTerm: Glossary Term
glossaryText: "a human readable text that will be used as the [term](@) in a [definition](@) as presented in a [HRG](@)."
formPhrases: [ "glossaryterm{ss}", "glossary-term{ss}" ]
---

# Glossary Term

A **Glossary Term** is a human readable text that will be used as the [term](@) in a [definition](@) as presented in a [HRG](@).

When a [HRG](@) is generated, it is good practice that its entries consist of a [glossary term](@) and a description; if the corresponding [MRG entry](@) does not have a value in its `glossaryTerm`-field, it may be derived from other fields, in particular, the `term`-field (which must always exist).

It is distinct from the `term` field (as found, e.g., in an [MRG entry](@) or [header](@) of a [curated text](@)), in that it may have some additions. For example, a [term](@) of type [relation](@), such as `participant`, might need to be represented as `Participant (of an organization)`, or `Participant (of a transaction)` or similar.

