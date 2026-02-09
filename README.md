# home-assignment-1
# Q1 — Regex (Python re)

All Q1 tasks are implemented as separate scripts and print matches for test strings. 

## Q1.1 US ZIP codes

Goal: match 12345, 12345-6789, 12345 6789 with token boundaries. 

Regex: r"\b\d{5}(?:[- ]\d{4})?\b"

## Q1.2 Words that do NOT start with a capital letter

Goal: words may contain internal apostrophes/hyphens (don’t, state-of-the-art). 

Regex: r"\b(?![A-Z])[A-Za-z]+(?:[’'][A-Za-z]+|-[A-Za-z]+)*\b"

## Q1.3 Numbers with sign/commas/decimals/scientific notation

Goal: extract numbers like -123, 1,234.5, 1.23e-4. 

Regex: r"[+-]?(?:(?:\d{1,3}(?:,\d{3})+)|\d+)(?:\.\d+)?(?:[eE][+-]?\d+)?"

## Q1.4 Email spelling variants

Goal: match email, e-mail, e mail, including en-dash –, case-insensitive. 

Regex: r"\be(?:mail|[-– ]mail)\b" with re.IGNORECASE

## Q1.5 “gooo” interjection with optional punctuation

Goal: match go, goo, gooo… as a word with optional ! . , ?. 

Regex: r"\bgo+\b[!.,?]?"

## Q1.6 Line ends with ? then only closers/spaces

Goal: lines ending with ? optionally followed by only closing quotes/brackets like )"”’] + spaces. 

Regex: r"\?[)\]\"”’']*\s*$" with re.MULTILINE

# Q2 — BPE (Byte Pair Encoding)
## Q2.1 Manual BPE (by hand)

Stored in: q2_bpe/q2_1_manual_bpe.txt 

Initial corpus counts

low ×5, lowest ×2, newer ×6, wider ×3, new ×2

Add end marker _ and initial vocabulary

Initial symbols include: {l, o, w, e, s, t, n, r, i, d, _}.

First 3 merges (summary)

Step 1: merge (e, r) → er (tie with (r, _), chose (e, r))

Step 2: merge (er, _) → er_

Step 3: merge (n, e) → ne (tie with (e, w) and (w, e))

After each step, the new token and updated vocabulary are listed in the text file.

Q2.1 Code (prints 3 steps automatically)

File: q2_bpe/q2_1_manual_bpe_code.py

Builds the toy corpus

Computes bigram counts

Executes 3 merges

Prints top pair, updated snippet (≥2 words), and vocabulary size

## Q2.2 Mini-BPE learner (code)

File: q2_bpe/q2_2_mini_bpe_learner.py 

Outputs:

top pair at each step

evolving vocabulary size

segmentation for: new, newer, lowest, widest, newestest (invented) with _

## Q2.2 Explanation (5–6 sentences)

Subword tokenization reduces OOV because unseen words can be decomposed into known subword pieces instead of becoming unknown tokens. Even if a word like “widest” was not in training, it can still be represented as character/subword sequences ending in _, preventing zero probabilities. BPE also reuses frequent patterns, reducing sparsity by sharing subwords across words. For example, BPE often learns er_ from “newer” and “wider,” aligning with the English “-er” suffix (comparative/agent-like function). This lets the model generalize to related forms while keeping the vocabulary smaller than word-level tokenization.

# Q 2.3 paragraph 

Natural language processing is messy, but tokenization makes it manageable. In this class we compare words, subwords, and characters, because real text contains typos and new terms. A good tokenizer learns frequent patterns such as learning, learned, and learner. Rare words like electroencephalography still become representable with subwords. That is why subword methods improve generalization and efficiency.

### Five most frequent merges (example output from this paragraph)

('s','_') -> s_ (count = 17)

('e','_') -> e_ (count = 10)

('e','n') -> en (count = 7)

('a','r') -> ar (count = 7)

('d','_') -> d_ (count = 7)

### Five longest subword tokens (final vocab)

words_ (len 6)

learn (len 5)

lear (len 4)

and_ (len 4)

ter (len 3)

### Segmentation of 5 words (includes rare + derived/inflected)

electroencephalography → e le c t ro en c e p h al o g r a p h y_

generalization → g en er al iz at i on _

learning → learn in g _

learned → learn e d_

tokenization → t o k en iz at i on _

## Brief reflection (5–8 sentences)

In this paragraph, BPE learned a mix of suffix-like tokens (s_, d_, e_), frequent letter clusters (en, er, al, iz, at, on), and even a short stem token (learn). For English, this often captures useful morphology: for example, learn + ed_ and learn + in + g_ share the same stem, which helps the model connect related forms. A major pro is that subwords reduce the OOV problem—rare words like electroencephalography can still be represented as known pieces instead of becoming unknown. Another pro is a smaller, reusable vocabulary that can generalize across many words (e.g., iz, at, on appear in multiple terms). A concrete con is that merges are frequency-driven, so tokens don’t always align perfectly with true morpheme boundaries and can look linguistically “weird.” Another con is that splitting words into many pieces can increase sequence length, which raises computation and can make downstream models slower.


## Q3. Bayes Rule Applied to Text

The slide’s decision rule is essentially:

[
c_{MAP}=\arg\max_{c\in C} P(c),P(d\mid c)
]

where (d) is the document and (c) is a class/label.

---

### 1) Explain what (P(c)), (P(d\mid c)), and (P(c\mid d)) mean

### **(P(c)) — Prior probability of the class**

This is how likely a class is **before** looking at the document.
Example: if 70% of training emails are “spam,” then (P(\text{spam})\approx 0.7).

### **(P(d|c)) — Likelihood of the document given the class**

This is how likely it is to see this document’s words **assuming** the document truly belongs to class (c).
In Naive Bayes, we compute this from the word probabilities learned for that class (often as a product/sum of log-probabilities).

### **(P(c| d)) — Posterior probability of the class given the document**

This is what we actually want for classification: the probability the document belongs to class (c) **after** reading the document.
Bayes rule connects them:

[
P(c|d)=\frac{P(c),P(d| c)}{P(d)}
]

---

### 2) Why can the denominator (P(d)) be ignored when comparing classes?

Because (P(d)) is the **same number for every class** when the document (d) is fixed.

When choosing the best class:

[
\arg\max_{c} P(c\mid d)
= \arg\max_{c}\frac{P(c),P(d\mid c)}{P(d)}
]

Since dividing by the same constant (P(d)) does **not** change which class is largest:

[
\arg\max_{c}\frac{P(c),P(d\mid c)}{P(d)}
= \arg\max_{c} P(c),P(d\mid c)
]

So we ignore (P(d)) for classification (we only care which class wins, not the exact normalized probability).

## Q4 
Using **add-1 (Laplace) smoothing** for a multinomial NB likelihood:

[
P(w\mid -)=\frac{c(w,-)+1}{N_- + |V|}
]

Given:

* total tokens in negative class (N_- = 14)
* vocabulary size (|V| = 20)

### 1) Denominator (negative class)

[
N_- + |V| = 14 + 20 = 34
]

### 2) (P(\text{predictable}\mid -)) where (c(\text{predictable},-)=2)

[
P(\text{predictable}\mid -)=\frac{2+1}{34}=\frac{3}{34}\approx 0.0882
]

### 3) (P(\text{fun}\mid -)) where (c(\text{fun},-)=0)

[
P(\text{fun}\mid -)=\frac{0+1}{34}=\frac{1}{34}\approx 0.0294
]


