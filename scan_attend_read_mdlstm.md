## Scan, Attend and Read: End-to-End Handwritten Paragragh Recognition with MDLSTM Attention
https://arxiv.org/pdf/1604.03286

Introduced an end-to-end sequence model that eliminated one of the major pain points in traditional HTR (Handwrtitten Text Recognition)
 - Explicit Line Segmentation

### Core Idea: End-to-End Paragragh Recognition
Central goal of the paper is to transcribe an entire handwritten paragragh image, into a sequence of characters, 
 - without needing any prior, rigid steps like segmenting image into individual lines/words
 - for the goal to implicitly learn where the lines are and the correct reading order using **Attention**
 - traditionally, if segmentation fails, transcription fails
 - if this is achieved, robust to variations in line slant, skew, and complex page layouts

### Multi-Dimensional LSTM (MDLSTM) Encoder
Architcture is a classic Encoder-Decoder setup, bridged by an Attention mechanism

#### Encoder
Responsible for converting 2D images of paragagh into a sequence of highly descriptive feature vectors. Where MDLSTM comes in.

MDLSTM
 - extension of the standard LSTM network,
 - designed to process multi-dimensional data, like images,
 - by propagating in 4 directions (up, down, left, right)
 - role:
    - scanning entire paragragh image to extract rich, context-aware representation
    - unlike CNN --> 1D-RNN, MDLSTM ensures every feature vector knows the content surrounding it in both horizontal and vertical directions

#### Attention Mechanism
Model learns 2 things simultaneously
1) Where to look 
2) What character to read
And the paper uses a sequential attention-driven process to solve this, with 2 types of attention

#### Covert Attention (Vertical)
To segment lines and determine reading order

Uses MDLSTM output to focus its attention **vertically** on one text line at a time, learns a weighted mask that highlights row(s) corresponding to the current text line being transcribed.

This mimics the human act of reading line-by-line, after reading one line, the attention mechanism is trained to automatically shift its focus down to the next line in the paragragh.

#### Overt Attention (Horizontal)
To transcribe characters within selected line.

Ohce a single line is selected by covert attention, the overt attention operates Horizontally along that line and aligns decoded character with the sequential visual feature for that line.

Decoder is typically a standard RNN/LSTM that takes the context vector and its previous hidden state to output the next character in sequence, thereby "spelling" the word.

### Summary
 - Shift from multi-stage pipeline to a single, trainable Encoder-Decoder system
 - MDLSTM allows encoder to capture contextual information from both X and y axes of the image = richer feature representation of handwriting
 - Implicit Line Segmentation via the two different attention mechanisms
