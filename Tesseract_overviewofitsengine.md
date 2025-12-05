## Tesseract: an Overview
https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/33418.pdf

Was developed way back in 1984!! But while groundbreaking in its time, has architectural and algorithmic weaknesses that modern solutions aim to address.

### Two Pass recognition system
Document you are reading might use a font the main classifier has never seen.
Pass 1: 
 - OCR engine processes the document, word by word top to bottom
 - Character blobs first recognized using Static Classifier, pretrained on highly generalized set of fonts
 - After word is classified, use linguistic analysis (dictionary, grammar) to find best possible word string
 - if word is dictionary-valid, and has high confidence, Tesseract assumes recognition correct

On-the-fly trainin:
 - individual characters of high-confidence, correctly recognized words are immediately used as training samples for their **Adaptive Classifier**
 - as soon as adaptive classifier has seen 3 matching samples, it starts contributing to recognition results

Since its trained on actual specific font, it quickly becomes highly accurate for that font

Pass 2: Entire page scanned a second time
 - this time only focusing on words that were no recognised well in pass 1
 - low confidence/not in dict
 - adaptive classifier is now much more fully trained and now provides a much better classification for challenging characters

#### Normalisation
Static uses anisotropic, eliminating variations in aspect ration and stroke width, making is robustly generalized, but reduces information

Adaptive Classifier uses Isotropic "uniform in all directions", preserving true aspect ratio and stroke width.
 - Baseline/X-Height: uses two things to size and position the character
    - the text line's baseline (the line where letters sit)
    - x-height (height of lowercase lettes like 'x' or 'a')

### Some concepts that are still relevant now
Modern OCR still performs a sequence of high-level tasks:
1) Image Preprocessing & Binarization: Getting a clean image
    - binarisation = converting grayscale into binary image, two color values, based on set threshold
    - simplifies image data
2) Connected Component Analysis/Blob detection
    - Tesseract uses this to find characters
    - Modern models do this implicitly, but the goal is still to identify potential character regions
3) Layout analysis
    - Tesseract had to deal with Page layout, skew, and curved text, which are real world problems that any robust OCR system must handle
4) Segmentation (Character/Word Breaking)
    - over segmentation (broken characters) and under-segmentation (joined characters) are still problems
    - Modern methods handle tis implicitly (CTC/Transformers/attetion-based)
5) Classification (Character Recognition)
    - Matching the segmented character to a known class
    - paper distinguishes between a static classifier and an adaptive classifier (font-specific, faster),
        - highlights challenges of font variation
        - in tesseract, they use high-confidence classifications from the first pass to train a simple, font-specific "adaptive classifier" for future passes
        - Modern OCR often use LMs (Language Models) as a post-correction step eg. "rn"->"m"



