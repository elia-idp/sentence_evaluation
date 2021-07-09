# Initial Task

Create a AWS lambda function that evaluates and stores the sentences that users intereact while using chrome extension.

# Sentence Evaluation

Our chrome extension monitors user activity to offer personalised material in the exercises. The sentences that the users interact are evaluated in terms of intelligibility, informativeness and typicality and stored in the tables for other processes. If the sentence is found not intelligible, it is discarded. For  informativeness and typicality, a score is assigned to the sentence with respect to the word of interest (There is an existing SenEx repo which is used to evaluate and assign these scores to the sentences in COCA corpora). Another evaluation is to find out the Wordnet sense name of the meaning of the word of interest in the sentence. This spesific task is called word-sense disambiguation, WSD for future refenreces. 

#### Example input and results for sentence evaluation process

input:
  * sentence: string = "Today, I called Klara to let her know that I will be joining her party."
  * word of interest: string = "join_VERB" ("$lemma_$POS" -> $lemma: base form of a word, "POS": POS tag - {VERB, NOUN, ADJ, ADV} , we refer it as lempos.)
  * user_id: string = "newcomer"
  
 outcome:
 
  * result of senEx function = ('join', 'VERB', 'VBG', 0.71, (1.43, (('party', 'NOUN'), 'VERB+NOUN')))
     <pre>
      explanation of result of senEx function: 
      0.71 -> informativeness score, 
      1.43 -> typicality score,
      party_NOUN -> found collocation of the word in the sentence", 'VERB+NOUN' -> type of collocation
      </pre>
  * update on the tables
    <pre>
    Sentence (Table)                                            - New Entry
    id = Column(VARCHAR(36), primary_key=True)                  : "a random string" (unique ID generated to be a primary key)
    corpus_id = Column(VARCHAR(50), ForeignKey('corpora.id'))   : "WEB" (web is for each sentence parsed from extenstion so in this task it is constant)
    text = Column(Text)                                         : "Today, I called Klara to let her know that I will be joining her party." (text of sentence)
    length = Column(INTEGER)                                    :  15 (number of words)
    
    SentenceFeature (Table)                                                             - New Entry
    sentence_id = Column(VARCHAR(36), ForeignKey('sentence.id'), primary_key=True)      : "a random string"
    lempos = Column(VARCHAR(50), ForeignKey('lempos_feature.lempos'), primary_key=True) : "join_VERB" (word of interest)
    sense_id = Column(VARCHAR(50))                                                      : "0138013n" (Wordnet offset ID of "join_VERB" for its sense in sentence)
    collocate = Column(VARCHAR(50))                                                     : "party_NOUN" (collocate lempos)
    typicality = Column(INTEGER)                                                        : 1.43
    informativeness = Column(INTEGER)                                                   : 0.71
    lempos_ind = Column(INTEGER)                                                        : 13 (index of the lempos in the sentence)
    collocate_ind = Column(INTEGER)                                                     : 15 (index of collocate in the sentence)
    </pre>
### Additional task:
In order to protect users' privacy, the named entities such as person, city or country names in the sentences that the user clicks on a word should be replaced before further processing. 

#### Example for named entity replacement:
  inputs:
  * sentence = "Today, I called Klara to let her know that I will be joining her party.",
  * replacement names per category = {"person": "David H. Petraeus", "Hamid Karzai", "Donald Trump", "Marie Curie", "organization": "the London School of Economics", "Apple", "location": "Berlin", "Germany"}
  
  output:
  "Today, I called Marie Curie to let her know that I will be joining her party."
 
### Proposed Subtasks
* Decide on a named entity recognizer (NER): Spacy is one of the candidates for it, since it is heavily used in Elia. 
* Revision of replacement names per category: since the categories can vary between the named entity recognizers, after selecting the NER, the categories and replacement name for each category should be revised.
* Testing function with various type of named entities.
* Create a function to clean the sentences parsed from websites.
* Revise senEx repo for evaluation function. 
* Use cosine-lesk or another function for WSD to find out Wordnet sense offset ID of the word. 
* Create functions update the tables.
* Deployment of the function as a AWS lambda. 
