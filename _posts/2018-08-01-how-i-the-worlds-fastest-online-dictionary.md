---
layout: post
title: How I Built the World's Fastest Online Dictionary
tags: [japanese, dictionary, translation]
---

**tl;dr** I just released the world's fastest online English-to-Japanese dictionary here: [enja.kdict.org 速い英和辞典](http://enja.kdict.org/)

## Introduction

I love online dictionaries. In the past I used to use some offline dictionaries installed on my laptop loaded with some static dictionary files, including ones from Eijiro. However, over time I got tired of installing and re-configuring those offline dictionaries every time I change my laptop. Online dictionaries, particularly [ALC](http://alc.co.jp/), just do the job most of the time.

Except when they don't.

One of the biggest complaints that I had was their speed. Most online dictionaries are loaded with tons of ads and links that are not relevant to me ("Improve your English skills while being in Japan!" - well, no thanks, because staying in Japan is probably the worst way to improve your English skills :). It's their business model. By the time the page loads and I'm ready to enter the query, often I forget what I wanted to search for in the first place. The issue is even worse in the US, because some (if not most) of  English-to-Japanese online dictionaries are only served from Japan.

This is why I created [速い英和辞典](http://enja.kdict.org/). The name literally means "fast English-Japanese dictionary" in Japanese. It incrementally loads search results as you type. You'd be surprised how fast it is compared to most online dictionaries.

## Data

The dictionary data is the combination of Wiktionary and [EJDict](https://github.com/kujirahand/EJDict). 

Wiktionary is nice because it is released under creative commons and it has a reasonable amount of words. (Actually, the reason why I wrote [this article](http://cjka.io/2018/07/15/extracting-english-to-japanese-translation-from-wiktionary/) is exactly because I wanted to use it for this project.) However, later many users, including myself, found that Wiktionary is missing a lot of entries, including very basic words like "sting" and "inch", which is why I started looking for some other options. I ended up adding all words from EJDict (which is released under the MIT license - very nice). For the entries that are included both in Wiktionary and EJDict, Wiktionary data was used.

## How it works

The dictionary indexes words by splitting them into trigrams. For example, if the word you'd like to index is "hello", you split it into a list of trigrams (after adding beginning/end of word marks) as follows:

```
^he hel ell llo lo$
```

After that, you apply the hash function (the same one used in Java) to each of those trigram strings, calculate modulo the hash size (which is 512), and get the 'index number' for each trigram as below:

```
^he hel ell llo lo$
475 271 165 239 257
```

The final step for indexing is to split the index using those 'index numbers' into different files. A word is stored in multiple index files that correspond to those index numbers. For example, word 'hello' will be stored in index files 475.js, 271.js, 165.js, 239.js, 257.js, each of which is just a list of dictionary entries.

When you search, you do the same with your query. If the query is "shell", then you will get:

```
^sh she hel ell ll$
307 176 271 165 164
```

So you dynamically load 307.js, 176.js, ..., 165.js, and combine them. Notice two out of those five trigrams are common between "hello" and "shell". Because of this, when you search "shell," "hello" is included in the combined list of dictionary entries. The final ranking is computed by using a similarity metric (specifically, [Dice coefficient](https://en.wikipedia.org/wiki/S%C3%B8rensen%E2%80%93Dice_coefficient)) between the "query vector" and each "entry vector". A vector here is just a set of character unigrams, bigrams, and trigrams. 

The entire source code is in [this Github repo](https://github.com/mhagiwara/enja.kdict.org). 

## Infrastructure

As you saw above, everything about this dictionary is stored in static files, including the entire dictionary data that is split into 512 different JavaScript files. I simply used AWS S3 for serving static website, and CloudFront as the CDN. (partly because I've never used CloudFront for my personal projects). According to tons of feedback I received from Japanese twitter users, the dictionary blazingly fast even when accessed from Japan.

## Next Steps

As it is, the dictionary is just a proof-of-concept project that I hacked up within a couple of hours. Probably the biggest issue with the dictionary is the data. I still see some missing words and inadequate descriptions. I can probably add new words and fix those descriptions in my spare time manually.

Also, a lot of online dictionaries acquire new users through SEO by generating a lot of static/dynamic pages (one per word). I wonder if the same approach could work for this dictionary too...
