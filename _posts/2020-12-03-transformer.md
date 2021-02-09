---
title: Attention Is All You Need - Transformer
tags: ml nlp transformer
img_url: /assets/img/transformer.jpeg
layout: blog
---
### Thoughts
The paper, "Attention Is All You Need", is the first to present transformer architecture.
The attention mehanism that this architecture implements is capable of representing dependencies of words without a sequential structure like RNN.
This let the transformer architecture utilize the parallel computation with GPU. This is difficult to acheive in RNN models because of its sequential structure.
The paper states tthe three motifs of self-attention mechanism. One, computational complexity per layer. Two, parallelization. Three, path length for long-range dependencies for inputs. The table below shows the comparison with other networks.

<p align="center">
    <img src="/assets/img/transformer_table.png" alt="transformer rnn cnn comparison table"  width="75%"/>
</p>

### Architecture
I recommend looking back at the article for more details because this part will only stay simple focus more on the overview of the architecture.
Below is the diagram,
<p align="center">
    <img src="/assets/img/transformer_architecture.png" alt="transformer architecture"  width="40%"/>
</p>
Left part of the diagram is an encoder (multi-head self-attention mechanism + fully connected feed-forward network) and the right is a decoder (multi-head attention over output + multi-head attention connecting encoder and decoder + fully connected feed-forawrd).
#### Encoder
Starting from the bottom of the encoder, input embedding is done like many other networks into $$d_{model}$$ dimension.
Positional encoding is added to the embeddings. From the candidates of positional encodings, one using sine and consine functions is used. I need more background to understand this implementation.
Then, this gets fed into _multi-head attention_ or scaled dot-product attention layer. This layer is the highlight of the paper, and used in both encoder and encoder and even encoder-decoder in which encoder connects to decoder. 
Difference between multi-head attention and scaled dot-product attention is simply the latter is the single-head version of the former.
We can use parallelization in this step by using multi-head instead.
Here is a diagram of multi-head attention mechansim.

<p align="center">
    <img src="/assets/img/attention_diagram.png" alt="attention diagram"  width="75%"/>
</p>

The idea is simply taking a dot product of _query_ vector and _key_ vector to decide the weight to put on the _value_ vector. This shows the depencies of values. 
Then the result gets fed into a feed-forward network with one hidden layer with ReLU activation.

#### Decoder
Input embedding and positional encoding is identical to an encoder. Also, a feed-forward network after the attention mechanism layer is identical. However, the attention mechanism layers for decoder have some subtle difference. Self-attention layer attends up to the current point which means position after the current point needs to be masked.
This masking stage is shown in the diagram as "Mask (opt.)" and mask is done by putting $$-inf$$ to the position after the current point.
Attention layer for connecting encoder and decoder is different to other attention layers in a sense that _query_ comes from the decoder step and _key_ and _value_ comes from the encoder step. 

The overview of transformer ends here. It was a great fun reading the paper. In addition to the link to the paper, I leave a link of YouTube video that explains the structure in much more detail with visuals.



_Paper: <https://arxiv.org/pdf/1706.03762.pdf>_

_YouTube: <https://www.youtube.com/watch?v=4Bdc55j80l8>_