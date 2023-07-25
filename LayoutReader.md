# [LayoutReader: Pre-training of Text and Layout for Reading Order Detection](https://arxiv.org/abs/2108.11591)
The authors propose a  deep learning model for reading order detection using a dataset created from WORD documents



### Proposed Method : 
![](https://hackmd.io/_uploads/BkA74k692.png)



- The authors build a dataset using WORD files in DocX format and extract the reading order from the XML metadata, to obtain the bounding boxes for the words the file is converted to PDF and a PDF parser is used(PDF metamorphosis net).
- They propose LayoutReader a seq-to-sew model which uses both textual and layout information.
- The model uses LayoutLM as the encoder with a modified decoder 

**Encoder:**
- The encoder packs source and target segments into contiguos input sequence of LayoutLM and designs a self attention mask.
- The attention is applied such that source segments can attend to each other while target segment cant attend to rightward context(basically inside a segment there is bidirectional attention but between segments the tokens can only see past/left tokens)

**Decoder:**
- The decoder uses the source sequence and calculates a probability as follows(softmax variant)
![](https://hackmd.io/_uploads/rynqn169n.png)

### Experiments : 
- The datasets used for training is ReadingBank the dataset created by the authors
- There are three tasks; reading order detection, input order study and adaptation to OCR engines.
- Two separate variants are also trained for LayoutReader, one text only and the other layout only.
- The metrics used are avg page-level BLEU and avg relative distance.
- The LayoutReader achieves SOTA results and the variants results show that layout information is more important for reading order detection