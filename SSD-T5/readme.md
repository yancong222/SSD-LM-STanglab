# Demo

## Installation

Install from HuggingFace transformers using:

```from transformers import T5Tokenizer, TFT5Model```

## Supported Functionality

- Extract word representations from Contextualized Word Embeddings
- Extract utterance representations from Contextualized Paragraph (long text) Embeddings
- Score sequences using language model scoring techniques, including masked language models following [Raffel et al. (2019)](https://arxiv.org/abs/1910.10683).


## Examples

1. Extract word representations from contextualized word embeddings:

```py
def get_last_hidden_state(sent):
  inputs = tokenizer.encode(sent, return_tensors="tf")  # Batch size 1
  outputs = model(inputs, decoder_input_ids=inputs)
  last_hidden_states = outputs[0]  # The last hidden-state is the first element of the output tuple
  return last_hidden_states
  
sentence = "my dog is cute"

get_last_hidden_state(sentence).shape 
''' 
TensorShape([1, 5, 512])
'''
```

# Progress

1. _ongoing_
    - cosine similarity using paragraph (long text) embeddings
    - cross-entropy loss using static strategy
        -  TLC factors
        -  group difference
2. _brainstorm_
    - cross-entropy loss using moving window strategy
3. _done_
    - cosine similarity using utterance embeddings
        -  TLC factors: 
            -  disorganized speech is positively correlated with similarity [**clinical data**: Remora]
            -  null effects regarding impaired expressivity [**clinical data**: Remora]
        -  group difference: lower similarity in SSD than in HV
        - TLC individual items: word approximation

           
