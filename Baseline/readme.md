**Benchmarking**

**Source:**

1. _Latent Semantic Analysis_
    - Details
      - Bedi: cosine similarity of adjacent and skip-one automatically parsed sentences. Convex hull modeling. 
      - Corcoran: k-level measurements, min/max/90th%/SD of k-level coherence. 
      - Elvevag: moving windows relative to interviewer prompt
    - Findings
      - Bedi/Corcoran: Predictive of conversion to psychosis. Highest performing variables: Coherence max, min, variance + POS possessive pronouns. 
      - Elvevag-higher n windows demonstrate greater gap in coherence (6-8 words) TD vs. non
2. _Coherence/Tangentiality_
    - Details
      - Embeddings: LSA + Glove + Word2Vec + Sent2Vec; 
      - Sentence: mean vector + TD-IDF + SIF; 
      - Models: Tangentiality (vs. interviewer) + Coherence (within subject)
    - Findings (Iter et al, 2018)
      - Word2Vec+SIF+incoherence model; 
      - LSA + TFIDF+Tangentialtiy; 
      - (Glove or Word2Vec) + SIF + Tangentiality
3. _Coherence_
    - Details (Coherence calcualtions)
      - Sequential, Gap, static centroid vs. cumulative centroid. 
      - +/- lemmatization. 
      - LSA vs. skipgram with negative sampling (SGNS). 
      - Mean vs. minimum values.
    - Findings (Xu et al, 2020)
      - Minimum coherence > mean coherence. 
      - Both centroid-based metrics better than sequential or gap. 

**Ongoing:**

a. GloVe ngram cosine
    
b. NLTK.lm ngram perplexity
    
    
