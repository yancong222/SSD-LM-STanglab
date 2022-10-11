## Installation

Install from OpenAI API using:

```pip install --upgrade openai```

## Supported Functionality

- Extract word representations from Contextualized Word Embeddings
- Extract sentence representations from Contextualized Paragraph Embeddings
- Score sequences using language model scoring techniques [Brown et al. (2020)](https://arxiv.org/abs/2005.14165).


## Examples

Extract adjacent sentence pairs similarity from contextualized sentence embeddings:

```py
      # encode sentences to get their embeddings
      sentence = "I see you"
      embedding = get_embeddings(sentence, engine = 'your engine')
      embedding 
      # compute similarity scores of two embeddings
      result = cosine_similarity(embedding1, embedding2) 
      print("Similarity score:", result)
''' 
0.7445092103171302
'''
```

# Progress

1. _ongoing_
    - perplexity: derive perplexity with stride > 3
2. _brainstorm_
    - moving windows and embeddings
3. _done_
    - cosine similarity
        -  TLC factors: 
            -  disorganized speech is positively correlated with similarity [**clinical data**: Remora]
            -  weak effects regarding impaired expressivity [**clinical data**: Remora]
        -  group difference:
            - Standard Deviation (SD) can better discern HV and SSD
            - Mean is not as good
        - benchmarking with GloVe embeddings
    - derive perplexity with stride = 3
        - TLC factors: 
            - disorganized speech is positively correlated with perplexity [**clinical data**: YT passage]
            - impaired expressivity is weakly positively correlated with perplexity [**clinical data**: YT passage]
        - group difference:
            - Statistically significant group effects
            - Higher similarity in SSD
            - Lower similarity in HV
