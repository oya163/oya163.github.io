---
title: "How to preprocess Nepali language"
layout: post
date: 2018-11-01 17:16
image: /assets/images/markdown.jpg
headerImage: false
star: false
category: blog
author: oyashi
description: Simple techniques to preprocess Nepali language
---


## Introduction:

This blog is about simple techniques to preprocess Nepali dataset or corpus. Here you will learn how to preprocess Nepali dataset/corpus before running your machine learning algorithm. This will be more technical but for more theoretical portion, you can always refer to this [blog][1].

Since, this [dataset] [2] is scrapped directly from Nepali news portal site. There is a high chance of having bogus unicode characters like 'ZERO WIDTH SPACE', 'TABS', 'NEW LINE', 'ZERO WIDTH JOINER', 'ZERO WIDTH NON-JOINER', 'UTF-8 BOM' and much more. Also, we might want to remove Numbers (१२३), Symbols($/%+) and Punctuations(,|;). Therefore, these characters should be removed for corpus creation or dataset preparation, as these are kind of garbage characters which does not constitute much in improving machine learning models.

Here the folder structure is in the following format:

    ./raw
      /Auto
          /1448882460.txt
          /1449985980.txt
      /Bank
          /1450165740.txt
          /1450165980.txt
      /Blog
          /1449293640.txt
          /1449557640.txt

Hence, we will walk through each subfolders and get contents of each file and create a dataset in <label, data> format.

First, we create a lookup table to store unicode value of all unnecessary characters that includes punctuations, BOM, newline, tabs, symbols and others.
[Reference] [4]

    table = dict.fromkeys(i for i in range(sys.maxunicode)
                        if un.category(chr(i)).startswith(('P','N','S','Cf','Cn','Cc'))
                        and i != 45)


For example:<br>
P - any kinds of punctuation<br>
N - any kinds of number<br>
S - any kinds of symbol<br>
Cf - Other, Format (ZERO WIDTH SPACE, ZERO WIDTH NON-JOINER)<br>
Cn - Not assigned, Format<br>
Cc - Other, Control category (tab)<br>
45 - represents HYPHEN-MINUS, we do not want to remove it for now because of words like आ-आफ्नो, प्रचार-प्रसार etc.

Secondly, we read each file with encoding 'utf-8-sig' due to presence of BOM characters in the file. And, translate with the above dictionary table in order to remove unnecessary characters all at once.

    fp = open(curr_file, encoding='utf-8-sig').read()
    fp = fp.translate(table)


Moreover, we use regex to carefully remove HYPHEN-MINUS characters from the text. Since, we do not want to remove hyphen that's between the words like आ-आफ्नो, प्रचार-प्रसार, but we might want to remove from नेपाल- का, -लागे -भन्दै, where there is space either before or after hyphen.

    fp = re.sub(r"(?<!\w)[-]|[-](?!\w)",'',fp)


Additionally, we normalize the unicode characters so that canonical-equivalent ones will also have precisely the same binary representation. For example काठमाडा + ै + ं = काठमाडौं. Hence, डौं will be equivalent to डा + ै + ं. I have used NFC (Normalization Form C) which performs Canonical Decomposition, followed by Canonical Composition, so that the resulting string is canonical equivalent to the original unnormalized text. For more [info] [5].

    final_msg = label, un.normalize('NFC', fp)


Finally, write the properly formatted data in our csv file.

    writer.writerow(final_msg)


Sample records in <label, data> format:

    1, मंसिर काठमाडौं  सरकारले सोमबारदेखि ड्राइभिङ लाइसेन्समा स्मार्ट कार्ड प्रविधि कार्यान्वयनमा ल्याएको छ
    1, मंसिर काठमाडौं  टाटा मोटर्सले ट्याक्सीका लागि उपयुक्त दुई मोडल नेपाली बजारमा भित्राएको छ
    1,रवीन्द्र घिमिरे मंसिर काठमाडौं  चीन सरकारले नेपाललाई अनुदानमा  वटा विद्युतीय बस दिने भएको छ


It took approximately 90 seconds to process the dataset of size 71 MB on a normal i7 processor. This dataset is now ready to be shuffled and divided into train/val/test portion to feed it into machine learning algorithms.

Complete code is available on [github][6].

Many many thanks to [Nepali NLP Group][1], [sndsabin][2] and [Bal Krishna Bal, KU Professor][7]

Note: STOP WORDS removal is not done. Code might be rough.


[1]: http://nepalinlp.com/detail/processing-unicode-devnagari-in-python/ "NepaliPreProcessing"
[2]: https://github.com/sndsabin/Nepali-News-Classifier "Nepali News Dataset"
[3]: http://www.fileformat.info/info/unicode/index.htm "Unicode List"
[4]: https://stackoverflow.com/a/11066687/4595807 "SF1"
[5]: http://unicode.org/reports/tr15/#Canon_Compat_Equivalence "Unicode Normalization"
[6]: https://github.com/oya163/oya-nepali-nlp "github"
[7]: http://ku.edu.np/cse/faculty/bal/ "bkb"
