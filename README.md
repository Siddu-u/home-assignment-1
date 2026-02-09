# home-assignment-1
# Q1 — Regex (Python re)

All Q1 tasks are implemented as separate scripts and print matches for test strings. 

# Q1.1 US ZIP codes

Goal: match 12345, 12345-6789, 12345 6789 with token boundaries. 

Regex: r"\b\d{5}(?:[- ]\d{4})?\b"

# Q1.2 Words that do NOT start with a capital letter

Goal: words may contain internal apostrophes/hyphens (don’t, state-of-the-art). 

Regex: r"\b(?![A-Z])[A-Za-z]+(?:[’'][A-Za-z]+|-[A-Za-z]+)*\b"

# Q1.3 Numbers with sign/commas/decimals/scientific notation

Goal: extract numbers like -123, 1,234.5, 1.23e-4. 

Regex: r"[+-]?(?:(?:\d{1,3}(?:,\d{3})+)|\d+)(?:\.\d+)?(?:[eE][+-]?\d+)?"

# Q1.4 Email spelling variants

Goal: match email, e-mail, e mail, including en-dash –, case-insensitive. 

Regex: r"\be(?:mail|[-– ]mail)\b" with re.IGNORECASE

# Q1.5 “gooo” interjection with optional punctuation

Goal: match go, goo, gooo… as a word with optional ! . , ?. 

Regex: r"\bgo+\b[!.,?]?"

# Q1.6 Line ends with ? then only closers/spaces

Goal: lines ending with ? optionally followed by only closing quotes/brackets like )"”’] + spaces. 

Regex: r"\?[)\]\"”’']*\s*$" with re.MULTILINE

# Q2 — BPE (Byte Pair Encoding)
## Q2.1 Manual BPE (by hand)

Stored in: q2_bpe/q2_1_manual_bpe.txt 

Homework 1 (2)

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
