# Understanding semantic and pragmatic fluencies

The goal of this project is to use language models to improve accuracy of detecting language (dis)fluencies. There are three major components in this transformer pipeline: 

    - Get embedding vectors from the language models.
    - Calcuate semantic similarities scores with different word/utterance level strategies.
    - Derive statistics from the similarities scores.

## Large language models taxonomy overview:

click on the tree node to see details

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'fontSize': '100px'}}}%%
flowchart TD
    A[Transformer Language Models] ==> B[Does it have Encoder Transformer?]
    B == Yes ==> C[Does it also have Decoder Transformer?]
    C == Yes ==> D[[T5]] ====> G[(cross entropy loss)]
    G ==> J{{static}}
    G ==> K{{moving window}}
    D ====> P[(cosine similarity)]
    P ==> Q{{word embeddings}}
    P ==> R{{utterance embeddings}}
    C == No ==> E[[BERT]] ====> H[(logits)]
    H ==> L{{static}}
    H ==> M{{moving window}}
    E ====> S[(cosine similarity)]
    S ==> T{{word embeddings}}
    S ==> U{{utterance embeddings}}
    B == No ==> F[[GPT]] ====> I[(perplexity)]
    I ==> N{{stride = 3}}
    I ==> O{{stride > 3}}
    F ====> V[(cosine similarity)]
    V ==> W{{word embeddings}}
    V ==> X{{utterance embeddings}}
    
    click G href "https://github.com/yancong222/SSD-LM-STanglab/tree/main/SSD-T5/cross%20entropy%20loss" "Open this in a new tab" _blank
    click P href "https://github.com/yancong222/SSD-LM-STanglab/tree/main/SSD-T5/cosine%20similarity" "Open this in a new tab" _blank
    click H href "https://github.com/yancong222/SSD-LM-STanglab/tree/main/SSD-BERT/logits" "Open this in a new tab" _blank
    click S href "https://github.com/yancong222/SSD-LM-STanglab/tree/main/SSD-BERT/cosine%20similarity" "Open this in a new tab" _blank
    click I href "https://github.com/yancong222/SSD-LM-STanglab/tree/main/SSD-GPT/perplexity" "Open this in a new tab" _blank
    click V href "https://github.com/yancong222/SSD-LM-STanglab/tree/main/SSD-GPT/cosine%20similarity" "Open this in a new tab" _blank
    
    click D href "https://github.com/yancong222/SSD-LM-STanglab/tree/main/SSD-T5" "Open this in a new tab" _blank
    click E href "https://github.com/yancong222/SSD-LM-STanglab/tree/main/SSD-BERT" "Open this in a new tab" _blank
    click F href "https://github.com/yancong222/SSD-LM-STanglab/tree/main/SSD-GPT" "Open this in a new tab" _blank
    
    linkStyle 0 stroke-width:17px,fill:none,stroke:green;
    linkStyle 1 stroke-width:17px,fill:none,stroke:green;
    linkStyle 2 stroke-width:17px,fill:none,stroke:green;
    linkStyle 3 stroke-width:17px,fill:none,stroke:green;
    linkStyle 4 stroke-width:17px,fill:none,stroke:green;
    linkStyle 5 stroke-width:17px,fill:none,stroke:green;
    linkStyle 6 stroke-width:17px,fill:none,stroke:green;
    linkStyle 7 stroke-width:17px,fill:none,stroke:green;
    linkStyle 8 stroke-width:17px,fill:none,stroke:green;
    linkStyle 9 stroke-width:17px,fill:none,stroke:green;
    linkStyle 10 stroke-width:17px,fill:none,stroke:green;
    linkStyle 11 stroke-width:17px,fill:none,stroke:green;
    linkStyle 12 stroke-width:17px,fill:none,stroke:green;
    linkStyle 13 stroke-width:17px,fill:none,stroke:green;
    linkStyle 14 stroke-width:17px,fill:none,stroke:green;
    linkStyle 15 stroke-width:17px,fill:none,stroke:green;
    linkStyle 16 stroke-width:17px,fill:none,stroke:green;
    linkStyle 17 stroke-width:17px,fill:none,stroke:green;
    linkStyle 18 stroke-width:17px,fill:none,stroke:green;
    linkStyle 19 stroke-width:17px,fill:none,stroke:green;
    linkStyle 20 stroke-width:17px,fill:none,stroke:green;
    linkStyle 21 stroke-width:17px,fill:none,stroke:green;
    linkStyle 22 stroke-width:17px,fill:none,stroke:green;
    
```

## Word/token level and Utterance level coherence measurements:

    - K2:10: the word-to-word variability at K inter-word distances, with K ranging from 2 to 10
    - MV5/10: the average semantic similarity of each word in 5- or 10- words window
    - FOC: the first order cosine similarity of consecutive phrase vectors
    - SOC: the second order cosine similarity between phrase separated by another intervening phrase


```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'fontSize': '100px'}}}%%
flowchart LR
    A[Embedding Strategy] ==> B[Measurement unit]
    B == Word ==> C[Language Models]
    C == Baseline ==> D[[GloVe]] ====> G[(MV5/10: Coherence within a window i, i=5,10)]
    G ==> J{{min, max, mean, std}}
    G ==> K{{5%, 95%, median, IQR}}
    D ====> P[(K2:10: word_id vs. word_id+i, i=2:10)]
    P ==> Q{{min, max, mean, std}}
    P ==> R{{5%, 95%, median, IQR}}
    C == Transformer ==> E[[BERT T5 GPT]] ====> H[(MV5/10: Coherence within a window i, i=5,10)]
    H ==> L{{min, max, mean, std}}
    H ==> M{{5%, 95%, median, IQR}}
    E ====> S[(K2:10: word_id vs. word_id+i, i=2:10)]
    S ==> T{{min, max, mean, std}}
    S ==> U{{5%, 95%, median, IQR}}
    B == Utterance ==> F[[Language Models]] ====> I[(GloVe)]
    I ==> N{{FOC}}
    I ==> O{{SOC}}
    N ==> a{{min, max, mean, std}}
    N ==> b{{5%, 95%, median, IQR}}
    O ==> c{{min, max, mean, std}}
    O ==> d{{5%, 95%, median, IQR}}
    F ====> V[(BERT T5 GPT)]
    V ==> W{{FOC}}
    V ==> X{{SOC}}
    W ==> e{{min, max, mean, std}}
    W ==> f{{5%, 95%, median, IQR}}
    X ==> g{{min, max, mean, std}}
    X ==> h{{5%, 95%, median, IQR}}
    
    
    linkStyle 0 stroke-width:17px,fill:none,stroke:green;
    linkStyle 1 stroke-width:17px,fill:none,stroke:green;
    linkStyle 2 stroke-width:17px,fill:none,stroke:green;
    linkStyle 3 stroke-width:17px,fill:none,stroke:green;
    linkStyle 4 stroke-width:17px,fill:none,stroke:green;
    linkStyle 5 stroke-width:17px,fill:none,stroke:green;
    linkStyle 6 stroke-width:17px,fill:none,stroke:green;
    linkStyle 7 stroke-width:17px,fill:none,stroke:green;
    linkStyle 8 stroke-width:17px,fill:none,stroke:green;
    linkStyle 9 stroke-width:17px,fill:none,stroke:green;
    linkStyle 10 stroke-width:17px,fill:none,stroke:green;
    linkStyle 11 stroke-width:17px,fill:none,stroke:green;
    linkStyle 12 stroke-width:17px,fill:none,stroke:green;
    linkStyle 13 stroke-width:17px,fill:none,stroke:green;
    linkStyle 14 stroke-width:17px,fill:none,stroke:green;
    linkStyle 15 stroke-width:17px,fill:none,stroke:green;
    linkStyle 16 stroke-width:17px,fill:none,stroke:green;
    linkStyle 17 stroke-width:17px,fill:none,stroke:green;
    linkStyle 18 stroke-width:17px,fill:none,stroke:green;
    linkStyle 19 stroke-width:17px,fill:none,stroke:green;
    linkStyle 20 stroke-width:17px,fill:none,stroke:green;
    linkStyle 21 stroke-width:17px,fill:none,stroke:green;
    linkStyle 22 stroke-width:17px,fill:none,stroke:green;
    linkStyle 23 stroke-width:17px,fill:none,stroke:green;
    linkStyle 24 stroke-width:17px,fill:none,stroke:green;
    linkStyle 25 stroke-width:17px,fill:none,stroke:green;
    linkStyle 26 stroke-width:17px,fill:none,stroke:green;
    linkStyle 27 stroke-width:17px,fill:none,stroke:green;
    linkStyle 28 stroke-width:17px,fill:none,stroke:green;
    linkStyle 29 stroke-width:17px,fill:none,stroke:green;
    linkStyle 30 stroke-width:17px,fill:none,stroke:green;
    
```

## Pipeline code skeletons:
- Baseline: GloVe static embeddings demostration
```py
fname = get_tmpfile("your directory/glove.6B.300d.w2v.txt")
model = KeyedVectors.load_word2vec_format(fname)
glove = model.wv

def cosine_similarity(a, b):
    return np.dot(a, b) / (np.linalg.norm(a) * np.linalg.norm(b))
    
sentence1 = "He sold the shares back to the bank"
sentence2 = "The river flowed over the bank"
sentence3 = "A little girl walked by the river bank"

def calc_glove(sent):
    for w in sent.split(' '):
        if w == 'bank':
            return glove[w] 
sentence1 = "He sold the shares back to the bank"
sentence2 = "The river flowed over the bank"
sentence3 = "A little girl walked by the river bank"
bank1 = calc_glove(sentence1)
bank2 = calc_glove(sentence2)
bank3 = calc_glove(sentence3)
print(bank3.shape)
print(cosine_similarity(bank1, bank2))
print(cosine_similarity(bank1, bank3))
print(cosine_similarity(bank2, bank3))

'''
(300,)
1.0
1.0
1.0
'''
```


- BERT: word level coherence computation 
```py
  """
  Preprocesses text input in a way that BERT can interpret.
  """
  marked_text = "[CLS] " + text + " [SEP]"
  tokenized_text = tokenizer.tokenize(marked_text)
  indexed_tokens = tokenizer.convert_tokens_to_ids(tokenized_text)
  segments_ids = [1]*len(indexed_tokens)

  # convert inputs to tensors
  tokens_tensor = torch.tensor([indexed_tokens])
  segments_tensor = torch.tensor([segments_ids])

  """
  Obtains BERT embeddings for tokens, in context of the given response (list of sentences).
  """

  sentence1 = ["He sold the shares back to the bank"]
  sentence2 = ["The river flowed over the bank"]
  sentence3 = ["A little girl walked by the river bank"]
  
  sentences = sentence1 + sentence2 + sentence3
  
  context_embeddings = []
  context_tokens = []
  for sentence in sentences: 
    tokenized_text, tokens_tensor, segments_tensors = bert_text_preparation(sentence, tokenizer)
    list_token_embeddings = get_bert_embeddings(tokens_tensor, segments_tensors, model)
    tokens = OrderedDict()
    for token in tokenized_text[1:-1]:
      if token in tokens:
        tokens[token] += 1
      else:
        tokens[token] = 1
      token_indices = [i for i, t in enumerate(tokenized_text) if t == token]
      current_index = token_indices[tokens[token]-1]
      token_vec = list_token_embeddings[current_index]
      context_tokens.append(token)
      context_embeddings.append(token_vec)

  bank1 = context_embeddings[7]
  bank2 = context_embeddings[13]
  bank3 = context_embeddings[-1]
  print(bank3.shape)
  print(cosine_similarity(bank1, bank2))
  print(cosine_similarity(bank1, bank3))
  print(cosine_similarity(bank2, bank3))
''' 
torch.Size([768])
0.7267714
0.7151612
0.88328594
'''
```

- T5: word/sentence level coherence computation
```py
def get_last_hidden_state(sent):
  inputs = tokenizer.encode(sent, return_tensors="tf", add_special_tokens=False)  # Batch size 1 # add special tokens or not?
  outputs = model(inputs, decoder_input_ids=inputs)
  # The last hidden-state is the first element of the output tuple (c.f. Raffel et al 2020)
  last_hidden_states = outputs[0]  
  return last_hidden_states

def cosine_similarity(a, b):
    return np.dot(a, b) / (np.linalg.norm(a) * np.linalg.norm(b))

sentence1 = "He sold the shares back to the bank"
sentence2 = "The river flowed over the bank"
sentence3 = "A little girl walked by the river bank"

bank1 = get_last_hidden_state(sentence1)[0][-1]
bank2 = get_last_hidden_state(sentence2)[0][-1]
bank3 = get_last_hidden_state(sentence3)[0][-1]
print(bank3.shape)

print(cosine_similarity(bank1, bank2))
print(cosine_similarity(bank1, bank3))
print(cosine_similarity(bank2, bank3))

'''
TensorShape([512])
0.94073576
0.9394256
0.9653907
'''
```

```py
s1 = "hearty meal was devouring me"
s2 = "hearty meal was devouring me yesterday in the park"
s3 = "my dog is cute"

sim = 1 - scipy.spatial.distance.cosine(get_last_hidden_state(s1)[[0]][4], get_last_hidden_state(s2)[[0]][4]) 
nsim = 1 - scipy.spatial.distance.cosine(get_last_hidden_state(s2)[[0]][4], get_last_hidden_state(s3)[[0]][4]) 
print("similarity between s1 and s2: ", sim, " similarity between s2 and s3: ", nsim)

'''
similarity between s1 and s2: 0.9817931652069092  similarity between s2 and s3: 0.8603241443634033
'''
```

- GPT3: word level coherence computation
```py
def demo_calc_gpt3(sent): 
  temp = sent.split(' ') 
  input = ' '.join([i for i in temp if (i != '') & ('{' not in i) & ('}' not in i) & ('#' not in i)]) 
  vec = get_embeddings(input.split(' '), engine = 'text-similarity-babbage-001') 
  return vec

sentence1 = "He sold the shares back to the bank"
sentence2 = "The river flowed over the bank"
sentence3 = "A little girl walked by the river bank"

bank1 = demo_calc_gpt3(sentence1)
bank2 = demo_calc_gpt3(sentence2)
bank3 = demo_calc_gpt3(sentence3)
print(len(bank3[-1]))

print(cosine_similarity(bank1, bank2))
print(cosine_similarity(bank1, bank3))
print(cosine_similarity(bank2, bank3))

'''
2048
0.9999858467203809
0.9999858467203809
1.0
'''
```

```py
# Average semantic similarity of each word in 5- or 10- words window
def divide_chunks(l, n):
    # looping till length l
    for i in range(0, len(l), n): 
        yield l[i:i + n]
response = 'Mhm . I\'m a thirty five year old man who uh um , you know , is very technically inclined . um I uh tend to um like to be a s a jack of all trades and I find a lot of enjoyment in , you know , pursuing lots of hobbies uh at the same time .'
response_emb = get_embeddings(response, your_gpt3_engine) # get word embeddings, in context of the given response
word_embed_chunk = list(divide_chunks(response_emb, int(5))) # divide into unit-5 chuncks
print('GPT-3 response embeddings: ' response_emb)
chunk_temp_agg = [] # collect similarity for all the unit-5 chunk of the response
for chunck_id, word_embed in enumerate(word_embed_chunk): # loop each unit-5 chunk
    temp_agg = []
    for word_id, embed in enumerate(word_embed): # aggregate similarity of each word within that unit-5 chunk
        w1 = embed
        w2 = word_embed[word_id+1]
        temp = cosine_similarity(w1, w2)
        temp_agg.append(temp)
    temp_sim = np.average(temp_agg) # take average
    chunk_temp_agg.append(temp_sim) # collect similarities for that unit-5 chunk
sim = np.average(chunk_temp_agg) #take average; can also do other stats
print('Average semantic similarity in 5-words window: ', round(sim, 2))
'''
GPT-3 response embeddings: [[0.003884142730385065, -0.01114651095122099, -0.01787032000720501, -0.0008272163104265928, 0.004206461366266012, 0.014679774641990662, -0.03089362382888794, -0.006034293211996555, 0.012574504129588604, -0.018898475915193558, 0.015593690797686577, 0.02663412131369114, 0.02882099151611328, 0.0012168546672910452, -0.0025520287454128265, 0.010240755043923855, ...

Average semantic similarity in 5-words window: 0.78
'''
```
---------------------------------------------
## Credit
- ** Author: Yan Cong (yancong222@gmail.com)
- ** Program's development draws significantly insights from** Sunny X. Tang and Katrin H<span>&#228;</span>nsel
- ** This program is built up on** Andreas Pogiatzis (2019), Alberto Parola et al. (2022), Ashish Vaswani et al. (2017), Thomas Wolf et al. (2020), Tom Brown et al. (2020), and openai-python (https://github.com/openai/openai-python)
---------------------------------------------
## Note
- **Pipeline details** can be found in the [PDF file](https://github.com/yancong222/SSD-LM-STanglab/blob/main/SSDHV_SemanticSimilarity_Methods.pdf)
