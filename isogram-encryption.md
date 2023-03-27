<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"><title>Atbash-like ciphers</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet" type="text/css" href="style.css">
</head>

# Atbash-like ciphers with isograms

## Introduction
The first cipher I ever learned to use was the Atbash cipher, in primary school. I don't remember
where I heard about it, but it was incredibly neat how you could fold the English alphabet on
itself and suddenly have a way of turning readable text into a garbled mess that was easy to turn
back:
```
abcdefghijklm
zyxwvutsrqpon
```

This morning I wondered if there were any pairs of 13 letter isograms which didn't share any letters
with each other that could be used for the same purpose (but be "easier to remember" than the last
13 letters of the alphabet in reverse). After realising that might not be realistic, I also wondered
if there were any pairs of partitions of 13 unique letters (e.g. a 6 letter word + a 7 letter word)
that similarly didn't share any letters with each other. Felt like a fun thing to mess around with.

Because I'm doing this for fun, I'm not going to look up prior art which most definitely exists. I'm
not going to focus on performance when it comes to the search either; I'm sure there'll be a smart
way to use graph theory or something to make it faster.

## 13 letter isograms
We're going to use the [`words`](https://en.wikipedia.org/wiki/Words_(Unix)) file on Linux to get a
hopefully acceptable list of 13 letter words:
```bash
$ egrep '^[[:alpha:]]{13}$' /usr/share/dict/words > ~/13_letters.txt
```

Let's explore this file with the Python REPL now.
```pycon
>>> with open("13_letters.txt") as f:
...     words = [w.strip() for w in f.readlines()]
...
>>> words[:10]
['abbreviatable', 'abbreviations', 'abdominoscope', 'abdominoscopy', 'Aberdeenshire', 'abiogenetical', 'abiologically', 'abnormalising', 'abnormalities', 'abnormalizing']
```

We can see with `'Aberdeenshire'` that this list has capital letters in it, which will make it more
difficult to find isograms. Normalise the list:
```pycon
>>> words = [w.lower() for w in words]
>>> words[:10]
['abbreviatable', 'abbreviations', 'abdominoscope', 'abdominoscopy', 'aberdeenshire', 'abiogenetical', 'abiologically', 'abnormalising', 'abnormalities', 'abnormalizing']
```

Now to find which ones are isograms.
```pycon
>>> from collections import defaultdict
>>> unique_count = defaultdict(list)
>>> for word in words:
...     unique_count[len(set(word))].append(word)
...
>>> unique_count[13]
['amphigenously', 'brachydontism', 'bridgehampton', 'chromeplating', 'consumptively', 'copyrightable', 'courtezanship', 'dentosurgical', 'documentarily', 'embryoplastic', 'endolymphatic', 'forecastingly', 'hemidactylous', 'hydromagnetic', 'hypsometrical', 'lycanthropies', 'lycanthropize', 'metalworkings', 'misconjugated', 'multibranched', 'musicotherapy', 'mynpachtbrief', 'objurgatively', 'philydraceous', 'physicomental', 'pneumogastric', 'postneuralgic', 'preblockading', 'salpingectomy', 'semivoluntary', 'submetaphoric', 'subordinately', 'sulphocyanide', 'sulphozincate', 'troublemaking', 'unatmospheric', 'unblameworthy', 'uncompahgrite', 'uncopyrighted', 'undisprovable', 'unexorcisably', 'unmaledictory', 'unpredictably', 'unproblematic', 'unsympathized']
```

Will we get a disjoint pair?
```pycon
>>> for i, word in enumerate(unique_count[13]):
...     for word2 in unique_count[13][i+1:]:
...             if set(word).isdisjoint(set(word2)):
...                     print(f"disjoint pair: {word} {word2}")
...
>>>
```

Damn. We'll need to try using partitions of 13 letters next, which is harder. I'm getting pretty
hungry, so I'll do this part later.
</html>
