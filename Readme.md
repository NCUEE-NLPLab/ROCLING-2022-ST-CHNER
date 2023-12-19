# ROCLING 2022 Shared Task on Chinese Healthcare Named Entity Recognition (CHNER)
##### 2023/10/05 --revise
The goal of this shared task is to develop and evaluate the capability of a Chinese healthcare NER recognizer. A sentence containing at least one named entity is given as the input. The recognizer should predict the named entity’s boundaries and category for each given sentence. We use the common BIO (Beginning, Inside, and Outside) format for the NER task. The B-prefix before a tag indicates that the character is the beginning of a named entity and the I-prefix before a tag indicates that the character is inside a named entity. An O tag indicates that a character belongs to no named entity. We use the same entity types defined in the Chinese HealthNER Corpus (Lee and Lu, 2021). A total of 10 types are described for this Chinese healthcare NER task, and some examples are provided in Table 1. 

|  Entity Type (Tag)   | Description  |   Examples  |
|  :----:  | ----  | ----  | 
| Body (BODY)  | The whole physical structure that forms a personor animal including biological cells, organizations, organs and systems. | “細胞核” (nucleus), “神經組織” (nerve tissue), “左心房” (left atrium), “脊髓” (spinal cord), “呼吸系統” (respiratory system) |
| Symptom (SYMP)  | Any feeling of illness or physical or mental change that is caused by a particular disease. | “流鼻水”(rhinorrhea), “咳嗽” (cough), “貧血” (anemia), “ 失 眠 ” (insomnia), “ 心 悸 ” (palpitation), “耳鳴” (tinnitus) |
| Instrument (INST)  | A tool or other device used for performing a particular medical task such as diagnosis and treatments. | “血壓計” (blood pressure meter), “達文西手臂” (DaVinci Robots), “體脂肪計” (body fat monitor), “雷射手術刀” (laser scalpel) |
| Examination (EXAM)  | The act of looking at or checking something carefully in order to discover possible diseases. |  “聽力檢查”(hearing test), “腦電波圖” (electroencephalography; EEG), “核磁共振造影” (magnetic resonance imaging; MRI) |
| Chemical (CHEM)  | Any basic chemical element typically found in the human body. | “去氧核糖核酸” (deoxyribonucleic acid; DNA), “糖化血色素” (glycated hemoglobin), “膽固醇” (cholesterol), “尿酸” (uric acid) |
| Disease (DISE)  | An illness of people or animals caused by infection or a failure of health rather than by an accident. | “小兒麻痺症” (poliomyelitis; polio), “帕金森氏症” (Parkinson’s disease), “青光眼” (glaucoma), “肺結核” (tuberculosis) |
| Drug (DRUG)  | Any natural or artificially made chemical used as a medicine. | “阿斯匹靈” (aspirin), “普拿疼” (acetaminophen), “青黴素” (penicillin), “流感疫苗” (influenza vaccination) |
| Supplement (SUPP)  | Something added to something else to improve human health. | “維他命” (vitamin), “膠原蛋白” (collagen), “益生菌 ” (probiotics), “葡萄糖胺” (glucosamine), “葉黃素” (lutein) |
| Treatment (TREAT)  | A method of behavior used to treat diseases. | “藥物治療” (pharmacotherapy), “胃切除術” (gastrectomy), “標靶治療” (targeted therapy), “外科手術” (surgery) |
| Time (TIME)   | Element of existence measured in minutes, days, years. | “嬰兒期” (infancy), “幼兒時期” (early childhood), “青春期” (adolescence), “生理期” (on one’s period), “孕期” (pregnancy) |

The input is a sentence consisting of a sequence of character-based tokens including punctuation. The developed NER recognizer returns the corresponding BIO tags aligned to each token as the output. Example sentences are presented below.In Example 1, “肌肉 ” (muscle) and “骨骼 ” (skeleton) belong to the body entity type (denoted as BODY). “蛋白質 ” (protein) and “ 鈣 質 ” (calcium) are chemicals (denoted as CHEM). In Example 2, we can find a disease “胃食道逆流症” (gastroesophageal reflux disease) (denoted as DISE). 

## Example 1
* _input:_ 修復肌肉與骨骼罪狀要的便是
熱量、蛋白質與鈣質。
* _output:_ O, O, B-Body, I-Body, O, B-Body, I-Body, O, O, O, O, O, O, O, O, O, B-CHEM, I-CHEM, I-CHEM, O, B-CHEM, I-CHEM, O
## Example 2
* _input:_ 如何治療胃食道逆流症？
* _output:_ O, O, O, O, B-DISE, I-DISE, I-DISE, I-DISE, I-DISE, O

# Data Preparation
The Chinese HealthNER Corpus (Lee and Lu, 2021) was used as the training set. It includes 30,692 sentences with a total around 1.5 million characters or 91,700 words. The data was sourced from articles on websites that provide healthcare information, on-line health news and medical question/answer forums. After manual annotation, this corpus consists of 68460 named entities across 10 defined entity types. 

We use the existing named entities in the Chinese HealthNER Corpus as the query terms andto find the corresponding articles in Chinese Wikipedia (zh_TW version). The first paragraph in the wiki articles was segmented into sentences for manual annotation. Three graduate students majoring in electrical engineering were trained in the named entity tagging task, producing a Fleiss’ Kappa value of inter-annotator agreement of 89%. All annotators were asked to discuss differences and seek consensus. When agreement was reached, each annotator was then asked to process sentences individually. As a result, our constructed test set includes 3,205 sentences with a total of 118,116characters and 13,369 named entities.   

Table 2 shows detailed statistics of mutually exclusive training and test sets. The entity type distribution is similar in both the training and test sets. The most frequently occurring type was Body, followed by Symptom, Disease and Chemical, collectively accounting for about 83% of all named entity instances, with the remaining 6 types accounting for 17%.  

|  Entity Type    |  #Train (%)  |  #Test (%)  |
|  :----:  | ----  | ----  |
|  Body  |  26411 (38.58%)  |  5315 (39.76%)  | 
|  Symptom  |  12904 (18.85%)  |  1944 (14.54%)  | 
|  Instrument  |  1089 (1.59%)  |  250 (1.87%)  | 
|  Examination  |  2622 (3.83%)  |  207 (1.55%)  | 
|  Chemical  |  6834 (9.98%)  |  1718 (12.85%)  | 
|  Disease  |  10079 (14.72%)  |  2609 (19.52%)  | 
|  Drug  |  2225 (3.25%)  |  481 (3.60%)  | 
|  Supplement  |  1525 (2.23%)  |  183 (1.37%)  | 
|  Treatment  |  3108 (4.54%)  |  468 (3.50%)  | 
|  Time  |  1663 (2.43%)  |  194 (1.44%)  | 
|  Total  |  68460 (100%)  |  13,369 (100%)  | 
  

In addition, sentences in the training set may contain named entities or not, each with an average of 49.31 characters and 2.23 named entities. However, all sentences in the test set contained at least one named entity, each with an average of 36.85 characters and 4.17 named entities. In summary, the average sentence length is short in the test set, but named entity density is relatively high. 

* Chinese HealthNER Corpus (train/dev)  
<a href="https://github.com/NCUEE-NLPLab/Chinese-HealthNER-Corpus" target="_blank">https://github.com/NCUEE-NLPLab/Chinese-HealthNER-Corpus</a>
* ROCLING-2022 Shared Task (test, this repository)  
<a href="https://github.com/NCUEE-NLPLab/ROCLING-2022-ST-CHNER" target="_blank">https://github.com/NCUEE-NLPLab/ROCLING-2022-ST-CHNER</a> 


# Evaluation
Performance is evaluated by examining the difference between the machine-predicted and human-annotated BIO tags. Standard precision, recall and F1-score are the most typical evaluation metrics of NER systems at a character level, and are used here. If the predicted tag of a character in terms of BIO format was completely identical with the gold standard, the character in the testing instances was regarded as correctly recognized.Precision is defined as the percentage of named entities found by the NER system that are correct. Recall is the percentage of named entities present in the test set found by the NER system. The F1-score is the harmonic mean of precision and recall.
```bash
python turn_to_eval.py --truth truth.txt --prediction predict.txt 
python conlleval.py < eval.txt 
```

# Results
The policy of this shared task is an open test. Participating systems are allowed to use other publicly available data for this shared task, but the usage should be specified in their system description paper. Each team was allowed to provide at most three submissions during the evaluation period. Among ten registered teams, seven submitted their testing results, providing a total of 20 submissions, from which the submission with the best F1-score of each team was kept in the leaderboard for performance ranking. Table 3 summarizes the task testing results. 
|  Rank  |  Team  |  Precision (%)  |  Recall (%)  |  F1  |
|  :----:  | ----  | ----  |  ----  |  ----  |
|  1  |  **MIGBaseline** (Ma et al., 2022)  |  81.99  |  81.88  |  81.93  |
|  2  |  **SCU-MESCLab** (Yang et al., 2022) |  80.18  |  78.3  |  79.23  |
|  3  |  **crowNER** (Zhang et al., 2022) |  77.82  |  78.1  |  77.96  |
|  4  |  **YNU-HPCC** (Luo et al., 2022) |  77.22  |  78.15  |  77.68  |
|  5  |  **NERVE** (Lin et al., 2022) |  79.59  |  73.09  |  76.2  |
|  6  |  **NCU1415** (Feng et al., 2022)  |  74.56  |  72.81  |  73.68  |
|  7  |  **SCU-NLP** (Chiou et al., 2022)  |  64.72  |  77.92  |  70.71  |

# Citation
Please cite the following papers for ROCLING 2022 CHNER Datasets:

Lung-Hao Lee, Chao-Yi Chen, Liang-Chih Yu, and Yuen-Hsien Tseng. 2022. Overview of the ROCLING 2022 shared task for Chinese healthcare named entity recognition. In *Proceedings of the 34th Conference on Computational Linguistics and Speech Processing*, pp. 363-368.

@article{ROCLING 2022,  
author={Lung-Hao Lee, Chao-Yi Chen, Liang-Chih Yu, and Yuen-Hsien Tseng},  
title={Overview of the ROCLING 2022 Shared Task for Chinese Healthcare Named Entity Recognition},  
year={2022},  
conference={In Proceedings of the 34th Conference on Computational Linguistics and Speech Processing},  
pages={363-368}  
}

# References
Lung-Hao Lee, and Yi Lu. 2021. Multiple embeddings enhanced multi-graph neural networks for Chinese healthcare named entity recognition. IEEE Journal of Biomedical and Health Informatics, 25(7): 2801-2810.  
  
Hsing-Yuan Ma, Wei-Jie Li, and Chao-Lin Liu. 2022. MIGBaseline at ROCLING 2022 shared task: reports on named entity recognition using Chinese healthcare datasets. In *Proceedings of the 34th Conference on Computational Linguistics and Speech Processing*, pp. 356-362.
  
Tsung-Hsien Yang, Ruei-Cyuan Su, Tzu-En Su, Sing-Seong Chong, and Ming-Hsiang Su. 2022. SCU-MESCLab at ROCLING 2022 shared task: named entity recognition using BERT classifier. In  *Proceedings of the 34th Conference on Computational Linguistics and Speech Processing*, pp. 329-334.

Qiu-Xia Zhang, Te-Yu Chi, Te-Lun Yang, and Jyh-Shing Roger Jang. 2022. CrowNER at Rocling 2022 Shared Task: NER using MacBERT and Adversarial Training. In *Proceedings of the 34th Conference on Computational Linguistics and Speech Processing*, pp. 321-328.

Xiang Luo, Jin Wang, and Xuejie Zhang. 2022. YNU-HPCC at ROCLING 2022 shared task: a transformer-based model with focal loss and regularization dropout for Chinese healthcare named entity recognition. In *Proceedings of the 34th Conference on Computational Linguistics and Speech Processing*, pp. 335-342.
  
Bo-Shau Lin, Jian-He Chen, and Tao-Hsing Chang. 2022. NERVE at ROCLING 2022 shared task: a comparison of three named entity recognition frameworks based on language model and lexicon approach. In *Proceedings of the 34th Conference on Computational Linguistics and Speech Processing*, pp. 343-349.

Zhi-Quan Feng, Po-Kai Chen, and Jia-Ching Wang. 2022. NCU1415 at ROCLING 2022 shared task: a light-weight transformer-based approach for biomedical named entity recognition. In *Proceedings of the 34th Conference on Computational Linguistics and Speech Processing*, pp. 316-320.

Sung-Ting Chiou, Sheng-Wei Huang, Ying-Chun Lo, Yu-Hsuan Wu, and Jheng-Long Wu. 2022. SCU-NLP at ROCLING 2022 shared task: experiment and error analysis of biomedical entity detection model. In *Proceedings of the 34th Conference on Computational Linguistics and Speech Processing*, pp. 350-355.
