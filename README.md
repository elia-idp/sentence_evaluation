# named-entity-replacement


Our chrome extension monitors user activity to offer personalised material in the exercises. In order to protect users' privacy, the named entities such as person, city or country names in the sentences that the user clicks on a word should be replaced before further processing. This repo is for python module that recieves a sentence and returns the same sentence after changing the named entities to a pre-determined replacement names.


### Example:
  inputs:
  *sentence = "Today, I called Klara to let her know that I will be joining her party.",
  *replacement names per category = {"person": "David H. Petraeus", "Hamid Karzai", "Donald Trump", "Marie Curie", "organization": "the London School of Economics", "Apple", "location": "Berlin", "Germany"}
  
  output:
  "Today, I called Marie Curie to let her know that I will be joining her party."
 
 
### Subtasks
* Decide on a named entity recognizer (NER): Spacy is one of the candidates for it, since it is heavily used in Elia. 
* Revision of replacement names per category: since the categories can vary between the named entity recognizers, after selecting the NER, the categories and replacement name for each category should be revised.
* Testing function with various type of named entities.
* Deployment of the function as a AWS lambda. 
