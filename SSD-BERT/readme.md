# Demo

## Installation

Install from HuggingFace transformers using:

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
    - utterance level cosine similarity using paragraph (long text) embeddings
2. _brainstorm_
    - derive logits with moving window strategy
    - derive logits with static strategy
3. _done_
    - word/token level cosine similarity using utterance/sentence embeddings
        -  TLC factors: 
            -  disorganized speech (incoherence and inefficiency) is positively correlated with similarity [**clinical data**: Remora]
            -  null effects regarding impaired expressivity [**clinical data**: Remora]
        -  group difference:
            - Statistically significant group effects between SSD and HV
            - Conceptual replication of Xu et al (2020)
            - Static centroid method
