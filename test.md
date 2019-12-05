###### tags: `Tésis`

___

WBLA Notes
=====

# Notes on

###  Abstract:
- Proposal of model for analyzing attention mechanisms
- Apply it to BERT
- BERT attends:
    - delimiter tokens
    - positional offsets
    - broadly the whole sentence
    - patterns corresponding to notions of syntax and coreference
    - \* Heads on the same layer exhibit similar behaviors

### Intro:
- Study of attention maps

# Personal Others
### Conclusions and turnouts:
- A surprisingly large amount of BERT’s attention focuses on the deliminator token [SEP], which we argue is used by the model as a sort of no-op.
- Generally we find that attention heads in the same layer tend to behave similarly. 
- While no single head performs well at many relations, we find that particular heads correspond remarkably well to particular relations. We find heads that find direct objects of verbs, determiners of nouns, objects of prepositions, and objects of possessive pronouns with >75% accuracy. 
- To get a more overall measure of the attention heads’ syntactic ability, we propose an attention-based probing classifier that takes attention maps as input 

### Questions:
- What specific linguistic features do they learn?
- 
- 

### Preprocessing:
- A special token [CLS] is added to the beginning of the text and another to- ken [SEP] is added to the end. 
- 


# Experiment Structure:
### First Look

1) General Analysis
2) Probe each head for linguistic phenomena. It treats each head  as a classifier that, given a word as input, outputs the most-attended-to other word.

## 1. “Surface Level”
![](https://i.imgur.com/YRXWKP1.png)

### Setup

Dataset size: 1000
Dataset source: Wiki

Setup similar for pre-training BERT:
- at most 128 tokens.
- two paragraphs from wiki
- **`[CLS]<paragraph-1>[SEP]<paragraph-2>[SEP]`**

### Relative position

How often attention heads attend to next, previous or current token.
- Most: little attention on current
- Some specialize on next or previous, specially in early layers
- 4 heads in layers 2, 4, 7, 8: >50% attention on previous 
- 5 heads in layers 1, 2, 2, 3, 6: >50% attention on next 

### Separator Tokens

![](https://i.imgur.com/wFKTiLA.png)
Attention is interestingly attracted  to separator tokens: > 50% attention to [SEP], while 1.56% would be average attention (given [SEP] is repeated twice).

\* [CLS] and [SEP] are guaranteed to be present, and "." and "," are the most common character apart from the. Is attention biased by stochastic training?

**Idea: Correlate attention to token with appearance**

[SEP] could be used to aggregate paragraph information, hence the attention? Probably not; [SEP] doesn't attend to the paragraph tokens, but generally to [SEP]. It seems that, when a head is specialized in a linguistic phenomena (e.g. direct objects attend to their verbs), it attends to [SEP] when the phenomena is not present (non-nouns mostly attend [SEP]).
This is further backed by observing that changing one's attention from [SEP] don't affect the output much.

![](https://i.imgur.com/MFcMOFT.png)


### Focused vs Broad Attention

In lower layers, some heads attend with high entropy, at most 10% on any single word.

![](https://i.imgur.com/u38odoh.png)

## 2. Probing Individual Attention Heads

![](https://i.imgur.com/5iM0uUk.jpg)

### Method

They use word-word atttention maps, transforming their original token-token attention maps the following way:
- **to a split word:** sum up attention
- **from a split word:** mean attention

This conserves the property that attention from each word sums to 1.

### Dependency Syntax

#### Setup
They evaluate both "directions": head word attending to dependent and dependent attending to head word.
They add a simple baseline, based on a fixed shift.



# TODOs
- [ ] check tokenization and setup conditions on BETO
- [ ] How do you measure accuracy? If a concept is formed by many words, does a connection to any of those words count? (e.g. on `pobj` "of" attends Treasury and bonds, so "of" attends "Treasury bonds" because the sum of both attentions is the highest? one of them is?)
- [ ] Size of Wall Street Journal portion of the Penn Treebank annotated with Stanford Dependencies?
- [ ] Fixed shift? border cases?