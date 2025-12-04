## TrOCR: Transformer-based Optical Character Recognition with Pre-trained Models
https://arxiv.org/pdf/2109.10282

Introduced an established a pattern: Take a Vision Transformer (ViT) encoder, and a Text Transformer decoder, connect them via cross-attention, and fine-tune on paired data.

Traditionally, OCR systems were made of
 - CNN to "see"
 - RNN (like LSTM) to handle sequence of text
 - CTC Loss to align the two

TrOCR replaces all that with a standard transformer architecture for BOTH
 - image understanding 
 - text generation
treating image patches like language tokens

### Components: Encoder
Instead of a CNN it uses a Vision Transformes (ViT) (Eg. Data-Efficient Image Transformer (DEiT)/BERT Pre-Training of Image Transfomers (BEiT)). 

And instead of a sliding kernel in a CNN, TrOCR uses a fixed grid, eg.
```txt
384x384 image sliced into non overlapping 16x16 squares
Giving 576 patches.
```
Some architectural details
 - standardized input to 384x384
 - BEiT-Large had best accuracy, but DeiT-Small used for speed

### Components: Decoder
Instead of a LSTM/RNN, they used a transformer decoder, BUT
 - there are very few pre-trained Decoder models
 - They took RoBERTa (pre-trained text Encoder) and MiniLM
    - RoBERTa does not have cross-attention layers, only self attention (encoder only)
    - inserted randomly initialized Cross-Attention layers between RoBERTa layers

### Components: Head (Output)
Wordpiece level generation (BPE)
 - recap: predicts sub-words like "ing", "tion", "the" instead of individual characters

The encoder and decoder was not trained from scratch, they use a model alreadly pre-trained on ImageNet for the encoder, and a model pre-trained on text for the decoder.

### Data Strategy
Both encoder and decoder uses models already pretrained on massive image and text datasets providing a solid foundation.

For fine-tuning, TrOCR uses two types of data
1) Synthethic Data: Generated text rendered over various backgrounds and fonts, teaching the model diverse styles
2) Real-World Data
     - Annotated images from documents, signs, and scenes

### Metrics
#### Word Accuracy
Strict metric where a predicted word is only counted as correct if it **exactly** matches the ground truth word.

#### Character Recognition Rate / Character Error Rate
Measures the number of characters that are correct/incorrect. Often calculated using the levenshtein distance between predicted and ground truth text sequences.
