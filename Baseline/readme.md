**BENCHMARKING**

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
      - Models: Tangentiality (vs. interviewer) + Coherence (within subject) + Second order coherence (skip a sentence)
      - Strategy: Sliding windows 
    - Findings 
      - Iter et al, 2018:
        - Word2Vec+SIF+incoherence model; 
        - LSA + TFIDF+Tangentialtiy; 
        - (Glove or Word2Vec) + SIF + Tangentiality
      - Voppel et al (2021): 
        - Use language connectedness in SSD to classify SSD and HV using spontaneous speech;
        - Using a word2vec semantic space model across multiple window sizes 
      - Parola et al (under review):
        - Only one previous result, i.e., reduced second-order coherence in schizophrenia, generalized across the entire corpus.
3. _Coherence_
    - Details (Coherence calcualtions)
      - Sequential, Gap, static centroid vs. cumulative centroid. 
      - +/- lemmatization. 
      - LSA vs. skipgram with negative sampling (SGNS). 
      - Mean vs. minimum values.
    - Findings (Xu et al, 2020)
      - Minimum coherence > mean coherence. 
      - Both centroid-based metrics better than sequential or gap. 
4. _Transformer-based language models_
    - Details
      - Huang et al:
        - BERT for semantic 
        - ELECTRA for syntactic
        - TERA for acoustic
    - Findings
      - Huang et al:
        - Pre-trained BERT's embedding is the most crucial feature in all scales but PANSS General
        - Fine-tuned ELECTEA (on CTB8) has moderate impact on TLC and PANSS
        - TERA has a low contribution when predicting thought disorders, especially the positive symptom and general psychopathology items (emotions reflect more in sounds than texts)
        - Training the model on TLC and PANSS together can provide higher performances compared to training on each scale alone. 
 
**Ongoing:**

1. _GloVe ngram cosine_
    - Incoherence, Inefficiency, and Impaired Expressivity are negatively correlated with similarity [**Clinical data: YT passage**]
2. _NLTK.lm ngram perplexity_
    - Incoherence and Inefficiency are negatively correlated with similarity [**Clinical data: YT turn**]
    - Impaired Expressivity is positively/negatively correlated with similarity [**Clinical data: YT turn**]

    
    
