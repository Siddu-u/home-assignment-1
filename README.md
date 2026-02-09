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
