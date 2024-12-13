# This is my note about word frequency

## Write a bash script to calculate the frequency of each word in a text file words.txt

For simplicity sake, you may assume:

- words.txt contains only lowercase characters and space ' ' characters.
- Each word must consist of lowercase characters only.
- Words are separated by one or more whitespace characters.
Example:

Assume that words.txt has the following content:

```txt
the day is sunny the the
the sunny is is
```
Your script should output the following, sorted by descending frequency:

```txt
the 4
is 3
sunny 2
day 1
```

---
# Solution

```bash

declare -A word_count

while read -r line; do
    ((word_count[$line]++))
done < <(tr -s '[:space:]' '\n' < words.txt)
# In this line, the data of words.txt will be taken and manipulated. It will be strip all the repeat space and each word will lay on 1 line. The data will be put in to while. Each line is a word. Look at the word_count. with every word, it will be initiated if have not been existed, if exist, it will be count + 1. 


# With this, the for will loop through the word_count key, and echo it key and value | after that sort
for item in "${!word_count[@]}"; do
    echo "$item ${word_count[$item]}"
done | sort -k2nr

# the sort mean that with k, use the second column , compare as number and reversely
```