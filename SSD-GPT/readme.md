## Installation

Install from OpenAI API using:

```pip install sentence-transformers```

## Supported Functionality

- Extract word representations from Contextualized Word Embeddings
- Extract sentence representations from Contextualized Paragraph Embeddings
- Score sequences using language model scoring techniques


## Examples

Extract adjacent sentence pairs similarity from contextualized sentence embeddings:

```py
# encode sentences to get their embeddings
      embedding1 = model.encode(sentence1, convert_to_tensor=True)
      embedding2 = model.encode(sentence2, convert_to_tensor=True)
      # compute similarity scores of two embeddings
      cosine_scores = util.pytorch_cos_sim(embedding1, embedding2)
      print("Sentence 1:", sentence1)
      print("Sentence 2:", sentence2)
      print("Similarity score:", cosine_scores.item())
''' 
0.4197316508
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
