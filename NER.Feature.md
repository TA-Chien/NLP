# NER
## Recognition
*    Recognition is difficult partly because of the ambiguity of segmentation; we need to decide what’s an entity and what isn’t, and where the boundaries are.
*    Another difficulty is caused by type ambiguity.
*    A sequence classifier like an  MEMM/CRF(feature based)), a bi-LSTM( neural based), or a transformer is trained to label the tokens in a text with tags that indicate the presence of particular kinds of named entities.
*    The standard neural algorithm for NER is based on the bi-LSTM
*    Instead a CRF layer is normally used on top of the bi-LSTM output, and the Viterbi decoding algorithm is used to decode.

## Rule-based NER
*    1. First, use high-precision rules to tag unambiguous entity mentions.
*    2. Then, search for substring matches of the previously detected names.
*    3. Consult application-specific name lists to identify likely name entity mentions from the given domain.
*    4. Finally, apply probabilistic sequence labeling techniques that make use of the tags from previous stages as additional features.

## Relation Extraction 
*    In most standard information extraction applications, the domain elements correspond to the named entities that occur in the text, to the underlying entities that result from co-reference resolution, or to entities selected from a domain ontology. 
*    WordNet or other ontologies offer useful ontological relations that express hierarchical relations between words or concepts. For example WordNet has the is-a or hypernym relation between classes
        *    Giraffe is-a ruminant is-a ungulate is-a mammal is-a vertebrate ...
*    WordNet also has Instance-of relation between individuals and classes, so that for example San Francisco is in the Instance-of relation with city. Extracting these relations is an important step in extending or building ontologies.
*    main classes of algorithms for relation extraction 
        *    handwritten patterns
        *    supervised machine learning
        *    semi-supervised (via bootstrapping and via distant supervision) 
        *    unsupervised.
### Handwritten patterns
*    a hyponym is a word or phrase whose semantic field is included within that of another word
*    PER, POSITION of ORG:
        *    George Marshall, Secretary of State of the United States
*    PER (named|appointed|chose|etc.) PER Prep? POSITION
        *    Truman appointed Marshall Secretary of State
*    PER [be]? (named|appointed|etc.) Prep? ORG POSITION
        *    George Marshall was named US Secretary of State

### Relation Extraction via Supervised Learning
*    A fixed set of relations and entities is chosen, a training corpus is hand-annotated with the relations and entities, and the annotated texts are then used to train classifiers to annotate an unseen test set
*    For the feature-based classifiers like logistic regression or random forests the most important step is to identify useful features.

### Embeddings 
*    Can be used to represent words in any of these features. Useful named entity features include
        *    Named-entity types and their concatenation
(M1: ORG, M2: PER, M1M2: ORG-PER)
        * Entity Level of M1 and M2 (from the set NAME, NOMINAL, PRONOUN)
M1: NAME [it or he would be PRONOUN]
M2: NAME [the company would be NOMINAL]
        * Number of entities between the arguments (in this case 1, for AMR)
*    syntactic structure
        * Base syntactic chunk sequence from M1 to M2
NP NP PP VP NP NP
        *    Constituent paths between M1 and M2
NP ↑ NP ↑ S ↑ S ↓ NP
        *    • Dependency-tree paths
Airlines$←_{sub_{j}}$ matched ←comp said $→{sub_{j}}$ Wagner
*  bi-LSTM model with word embeddings as inputs and a single softmax classification of the sentence output as a 1-of-N relation label. 
*  Because relations often hold between entities that are far part in a sentence (or across sentences), it may be possible to get higher performance from algorithms like convolutional nets or chain or tree LSTMS
## Extracting Times
*    Temporal Expression Extraction
        *    Absolute time  : Map to an calendar dates
        *    Duration : Spans of time at a time varying levels
        *    Relative : Map to particular times through some other reference point
                *    temporal lexical
                        *    Noun : morning, noon, night, winter, dusk, dawn
                        *    Proper Noun : January, Monday, Ides, Easter, Rosh Hashana, Ramadan, Tet
                        *    Adjective : recent, past, annual, former
                        *    Adverb : hourly, daily, monthly, yearly
*    Typical features used to train IOB-style temporal expression taggers.
        *    Token : The target token to be labeled
        *    Tokens in window : Bag of tokens in the window around a target
        *    Shape : Character shape features
        *    POS : Parts of speech of target and window words
        *    Chunk tags : Base-phrase chunk tag for target and words in a window
        *    Lexical triggers : Presence in a list of temporal terms
* Temporal Normalization
    * The first temporal expression in the text proper refers to a particular week of the year
    *  Durations are represented according to the pattern Pnx, where n is an integer denoting the length and x represents the unit, as in P3Y for three years
    *  Most current approaches to temporal normalization are rule-based
    
                         