---
title: Korean Hate Speech Detection
excerpt: Korean Hate Speech Detection In Cross-lingual Transfer Learning Embedding Models
layout: single
permalink: /korean-hate-speech-detection/
author_profile: true
header:
  image: /assets/images/project/korean-hate-speech-detection-poster.png
  teaser: /assets/images/project/korean-hate-speech-detection-poster.png
sidebar:
  nav: "sidebar"
---

## Outline

Korean Hate Speech Detection In Cross-lingual Transfer Learning Embedding Models for CS470, Introduction to Artificial Intelligence

[<i class='fa fa-address-book' aria-hidden='true'></i> Github](https://github.com/Byunk/Korean-Hate-Speech-Detection){: .btn .btn--info}
[<i class='fa fa-address-book' aria-hidden='true'></i> Poster](/assets/images/project/korean-hate-speech-detection-poster.pdf){: .btn .btn--info}

## Team Members

[Minsun Kim](https://github.com/min-sunnying)

[Jaemin Jun](https://github.com/jjmcoconut)

[Jeongjun Lee](https://github.com/boingkiri)

[Kyungho Byoun](https://github.com/byunk)

[Taegyeom Kim](https://github.com/jas03006)

## Motivation

Offensive language causes a great deal of conflict between people of different political ideologies, genders, races, and generations. Recent research focuses heavily on offensive datasets in English, as there are numerous offensive datasets in English available on the Internet.

Numerous studies attempt to solve this issue by training the model using both English and the target language. This is referred to as the cross-lingual language model, or XLM for short. The advantage of XLM is that we can discover the model's latent space using a large number of English datasets.

We use [Korean Hate Speech Dataset](https://github.com/kocohub/korean-hate-speech) for training and evaluation, and we set [Kaggle Competition](https://www.kaggle.com/competitions/korean-hate-speech-detection/overview) as a benchmark.
## Relative Works

BERT [Jacob Devlin et al., 2019](#references) and its derivatives has shown novel improvements in most NLP tasks in recent. In hate speech, there are automatic hate speech detection approaches and BERT achieve 7-8% improvement compared to previous system [MacAvaney S et al., 2020](#references). Kaggle competitions are actively held by big tech business such as Google and Meta and the leading model achieve about 80% accuracy in average. Recently, RoBERTa [Yinhan Liu et al., 2019](#references) is frequently used and show high performance.

The evident challenge of studies on this topic is how to deal with different languages. Cross-lingual embedded models, XLM-R [Conneau et al., 2019](#references), are suggested to take advantage of profoundness of English sources [Ranasinghe and Zampieri, 2020](#references). It explain XLM-R are exploited to deal with non-English languages without performance degradation. For Korean, there is single active Kaggle competition with Korean Hate Speech Dataset [Jihyung Moon et al., 2020](#references)

## Methods & Experiment

{% include figure image_path="/assets/images/project/korean-hate-speech-detection-poster-experiment.png" caption="Methods overview" %}


> Hyperparameter Tuning (XLM-R)

-   Cross-lingual transfer learning with relative position encoding

-   Adapting morpheme tokenizer of Korean

-   Data Augmentation

-   Various Pretrain methods

## Result

### **Overview**

|        Model         |            Evaluation Result             |
| :------------------: | :--------------------------------------: |
| Kaggle Leading Group |                  0.677                   |
|  Multi-lingual BERT  |                  0.548                   |
|        KoBERT        |                  0.604                   |
|      **XLM-R**       | **<span style="color:red">0.690</span>** |

<br />

### **Hyperparameter Tuning**

|                             Model                             |            Evaluation Result             | F1 Score |
| :-----------------------------------------------------------: | :--------------------------------------: | :------: |
|                          XLM-R Base                           |                  0.650                   |  0.641   |
|          XLM-R Base + Relative Positional Embedding           |                  0.555                   |  0.533   |
|                 XLM-R Base + English-pretrain                 |                  0.618                   |  0.612   |
| XLM-R Base + English-pretrain + Relative Positional Embedding |                  0.606                   |  0.593   |
|                XLM-R Large + English-pretrain                 |                  0.618                   |    -     |
|                XLM-R Large + Sandwich-pretrain                | **<span style="color:red">0.694</span>** |  0.691   |
|      XLM-R Large + Sandwich-pretrain + Korean-tokenizer       |                  0.690                   |  0.688   |

## Conclusion

XLM-R with cross-lingual transfer learning outperforms the other existing models in Korean hate speech detection.Base on this result, it is now possible to achieve higher performance using additional English datasets and advanced researches directly on Korean data.

To apply Korean tokenizer is the biggest challenge in this work. A linguistic awareness of given language makes a huge impact on performance specifically in Korean [Park et al., 2020](#references). In future work, it would be the first objective to fit existing Korean tokenizer such as Khaiii, Mecab into XLM-R model.

## Future Works

-   According to [Ranasinghe and Zampieri, 2020](#references), the results for other languages showed better results than Korean. Therefore, it seems that if training is performed by utilizing the Korean specific characteristics tailored to the Korean language, it will be able to show better results.

-   If the model is pre-trained with Japanese dataset, which is closer to the Korean, you will get better results. Similarly, better results could be achieved if it is pre-trained with another language which has more abundant data.

-   If the XLM-R-XL model produces better results, it may be right to assume that modeling a data-poor language using data-rich language datasets helps improve performance.

-   Data augmentation has been conducted by editing existing data text, but it seems that better results can be obtained by performing data augmentation using a large language model such as GPT.

## References

[1] Tharindu Ranasinghe and Marcos Zampieri. 2020. Multilingual Offensive Language Identification with Cross-lingual Embeddings. In Proceedings of the 2020 Conference on Empirical Methods in Natural Language Processing (EMNLP), pages 5838–5844. [Github Repository](https://github.com/TharinduDR/DeepOffense)

[2] Jihyung Moon, Won Ik Cho, Junbum Lee. 2020. BEEP! Korean Corpus of Online News Comments for Toxic Speech Detection

[3] MacAvaney S, Yao H-R, Yang E, Russell K, Goharian N, Frieder O (2019) Hate speech detection: Challenges and solutions. PLoS ONE 14(8): e0221152.

[4] Jacob Devlin, Ming-Wei Chang, Kenton Lee, & Kristina Toutanova. (2018). BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding. ArXiv: Computation and Language.

[5] Pires, T., Schlinger, E., & Garrette, D. (2019). How Multilingual is Multilingual BERT?. In Proceedings of the 57th Annual Meeting of the Association for Computational Linguistics (pp. 4996–5001). Association for Computational Linguistics.

[6] Matthew S. Dryer and Martin Haspelmath (2013). WALS Online. Max Planck Institute for Evolutionary

[7] Park, Kyubyong and Lee, Joohong and Jang, Seongbo and Jung, Dawoon. 2020. An Empirical Study of Tokenization Strategies for Various Korean NLP Tasks. ArXiv: Computation and Language.

## Review

{% include figure image_path="/assets/images/project/korean-hate-speech-detection-poster.png" caption="Poster" %}

{% include figure image_path="/assets/images/project/korean-hate-speech-detection.JPG" %}
