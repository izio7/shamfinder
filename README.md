# shamfinder

Shamfinder is a framework that detects homograph IDN.

This repository contains data code as described in our paper:

H. Suzuki, D. Chiba, Y. Yoneya, T. Mori, and S. Goto, "ShamFinder: An Automated Framework for Detecting IDN Homographs," Proceedings of the 19th ACM  Internet Measurement Conference (IMC 2019),  pp. 449--462, October 2019 [PDF]

https://dl.acm.org/authorize.cfm?key=N695002


## Homograph DB

Homograph DB is a part of shamfinder.

This is used when comparing the reference domain with the target domain.

This database is consist of two files (**simchar.json** and **confusables_v12-1_in_draft.json**).



### simchar.json

- how to create

1. multi_psnr.py

   Generate images(50*50 pixels) with embeddded character.

   Then I compare them brute force and calculate psnr by multi proccess.

   

2. json_summaraize.py

   This file adds the meta infomation and generates a json file.  The meta information is shown below.

    

   - black_pixels ... the number of black pixels

   - codepoint ... codepoint is the number which represent single character

   - decimal ... codepoint(decimal)

   - lang ... the name of the block to which the character belongs (<http://www.unicode.org/Public/UNIDATA/Blocks.txt>)

   - name ...character name decided by Unicode Consortium

   - similar_char ... list of characters similar to original character

   - delta  ... Indicates how many pixels are different from the base image

     

3. remove_dissimilar.py

   This file removes charactes with less than 10 black pixels and characters that differ from the base image by 7 or more pixels from json file.



As a result of executing these, the following file is obtained.

- example

```
"i": {
        "black_pixels": 15,
        "codepoint": "U+0069",
        "decimal": 105,
        "lang": "Basic Latin",
        "name": "LATIN SMALL LETTER I",
        "similar_char": [
            {
                "black_pixels": 19,
                "char": "ɨ",
                "codepoint": "U+0268",
                "decimal": 616,
                "lang": "IPA Extensions",
                "name": "LATIN SMALL LETTER I WITH STROKE",
                "delta": 4
            },
            
            			:
            			:
            			:
            			:
```







### confusable_v12-1_in_draft.json

This file shows characters that are similar to lowercase letters and numbers based on the confusables.txt provided by the Unicode Consortium. And it is made based on the following conditions.

1. These characters are allowed to use IDNA by **IDNA2008 and Unicode 12.0.0** (<https://tools.ietf.org/id/draft-faltstrom-unicode12-00.html>)
2. These characters have fonts in **unifont** (<http://unifoundry.com/unifont/index.html>)



- example

```
"b": [
        {
            "block": "Unified Canadian Aboriginal Syllabics",
            "char": "ᑲ",
            "decimal": 5234,
            "name": "CANADIAN SYLLABICS KA"
        },
        {
            "block": "Unified Canadian Aboriginal Syllabics",
            "char": "ᖯ",
            "decimal": 5551,
            "name": "CANADIAN SYLLABICS AIVILIK B"
        },
        
        					:
        					:
        					:
```

