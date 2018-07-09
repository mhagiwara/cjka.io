---
layout: post
title: How NOT to convert Simplified and Traditional Chinese
tags: [chinese, character]
---

**tl;dr** Do not use a random website or tool you find on the internet.

## Languages and Scripts

Chinese can be written in two different types of script, Simplified Chinese and Traditional Chinese. I often get asked about these scripts and would like to put together things to know when it comes to handling those scripts.

First off, these are *scripts*, not languages. These two are somewhat separate concepts. People often get confused by the fact that we often see both "Chinese (Simplified)" and "Chinese (Traditional)" in language menus on websites, but you can write, for example, Mandarin Chinese, in either of those two scripts. Mandarin Chinese, which is the official language for both mainland China and Taiwan, is usually written in Simplified Chinese in Mainland China (and also in Singapore) and in Traditional Chinese (and also in Malaysia) in Taiwan.

## N-to-N Mapping

Another pitfall regarding Simplified and Traditional Chinese is that there is often no simple 1-to-1 character mapping between those two scripts. This is something far more complex than e.g., English upper and lower cases. It is true that there is actually 1-to-1 mapping between many characters, most of which involve simplification of radicals as shown below, but for more common and high frequent characters, one simplified form corresponds to multiple traditional form.

Characters with 1-to-1 Mappings

| Simplified        | Traditional   |  Pinyin and Meanings |
| ----------------- | ------------- |  ------------------  |
| 马                | 馬             |  ma3 - horse         |
| 开                | 開             |  kai1 - to open      |
| 饭                | 飯             |  fan4 - food         |
| 语                | 語             | yu3 - language       |

Characters with 1-to-Many Mappings

| Simplified        | Traditional   |  Pinyin and Meanings |
| ----------------- | ------------- |  ------------------  |
| 发                | 發             |  fa1 - to send out   |
| 发                | 髮             |  fa4 - hair          |
| 干                | 乾             |  gan1 - dry          |
| 干                | 幹             | gan3 - to do         |
| 面                | 面             | mian4 - face         |
| 面                | 麵             | mian4 - noodle       |
| 后                | 後             | hou4 - after         |
| 后                | 后             | hou4 - queen         |

There are, though not many, characters with Many-to-1 mappings:

 | Simplified        | Traditional   |  Pinyin and Meanings |
| ----------------- | ------------- |  ------------------  |
| 着                | 著             |  zhe - (an aspect marker)   |
| 著                | 著             |  zhu4 - to write      |

## Wrong Way

The most common type of mistake that you may make when converting between those two scripts is to convert them character by character, by having a mapping table between them and replacing simplified to traditional characters (or the other way around) one by one. If you apply this method to the words below, it will result in wrong conversions. 

| Simplified        | Traditional (wrong)   |  Traditional (correct) | Pinyin and Meanings |
| ----------------- | ------------- |  ------------------  | ------------- |
| 头发               | 頭發             | 頭髮             | tou2fa4 - hair  |
| 干面               | 干面             | 乾麵             | gan1mian4 - dry noodle |

Unfortunately, if you use a random conversion tool you find on the Internet (for example, [this one](https://www.chinese-tools.com/tools/converter-simptrad.html), which is currently at the top result of Google search for "Simplified to Traditional Conversion"), there is a good chance that the tool is based on a simple but wrong algorithm like this. If you want to try out a tool for converting those scripts, at least try some simple words like the ones above to see if the tool is created by people who know what they are doing at all.

## Correct Way

Then, what *is* the correct way to convert between those two scripts? I said earlier that these two are scripts, not languages, but the conversion problem is such a complex issue that it is actually a good practice to treat this as a (somewhat simpler form of) machine translation problem between two different languages.

If you'd like to translate between English and Spanish, you wouldn't just want to simply grab a dictionary and replace one word at a time, would you? In order to "convert" them correctly, you need to actually understand what is written and rewrite the sentence if necessary. Similarly, when you are converting, for example, from traditional to simplified, you need to "understand" if this "著" is actually an aspect marker (which should be simplified to 着) or part of words like "著名" (which should be left as is) and make a decision accordingly.

Correctly converting those two scripts requires far more than just character-based replacement. Actually, my recommendation as of this writing is to _not_ to implement your own conversion. Google translate actually does a decent job converting between those two if you set the source language to Simplified Chinese and the target language "Traditional Chinese" (or vice versa).

If you still need to implement your own conversion, I'd recommend following [Wikipedia's conversion algorithm](https://zh.wikipedia.org/wiki/Help:%E4%B8%AD%E6%96%87%E7%BB%B4%E5%9F%BA%E7%99%BE%E7%A7%91%E7%9A%84%E7%B9%81%E7%AE%80%E3%80%81%E5%9C%B0%E5%8C%BA%E8%AF%8D%E5%A4%84%E7%90%86). I'll further discuss this in a future post. 

## Further Reading

A lot of the "pitfalls" I mentioned above are elegantly covered by this amazing paper by Halpern and Kerman: [The Pitfalls and Complexities of Chinese to Chinese Conversion](http://www.cjk.org/cjk/c2c/c2c.pdf). Although the paper is a bit outdated (for example, an average developer doesn't even need to know different character sets and encodings for Chinese - everyone uses Unicode/UTF nowadays) but is a great starting point if you'd like to understand this issue in further depth.
 