# Demo

## Installation

Install from Pypi using:

```pip install minicons```

## Supported Functionality

- Extract word representations from Contextualized Word Embeddings
- Score sequences using language model scoring techniques, including masked language models following [Salazar et al. (2020)](https://www.aclweb.org/anthology/2020.acl-main.240.pdf).


## Examples

1. Extract word representations from contextualized word embeddings:

```py
from minicons import cwe

model = cwe.CWE('bert-base-uncased')

context_words = [("I went to the bank to withdraw money.", "bank"), 
                 ("i was at the bank of the river ganga!", "bank")]

print(model.extract_representation(context_words, layer = 12))

''' 
tensor([[ 0.5399, -0.2461, -0.0968,  ..., -0.4670, -0.5312, -0.0549],
        [-0.8258, -0.4308,  0.2744,  ..., -0.5987, -0.6984,  0.2087]],
       grad_fn=<MeanBackward1>)
'''
```

# Progress

1. _ongoing_
    - cosine similarity using word embeddings
2. _brainstorm_
    - derive logits with moving window strategy
    - derive logits with static strategy
3. _done_
    - cosine similarity using utterance embeddings
        -  TLC factors: 
            -  disorganized speech is positively correlated with similarity [**clinical data**: Remora]
            -  null effects regarding impaired expressivity [**clinical data**: Remora]
        -  group difference:
            - Conceptual replication of Xu et al (2020)
            - Static centroid method
