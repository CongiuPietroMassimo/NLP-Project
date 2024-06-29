# Task 5: Rumor Verification using Evidence from Authorities

Given a rumor expressed in a tweet and a set of authorities (one or more authority Twitter accounts) for that rumor, represented by a list of tweets from their timelines during the period surrounding the rumor, the system should retrieve up to 5 evidence tweets from those timelines, and determine if the rumor is supported (true), refuted (false), or unverifiable (in case not enough evidence to verify it exists in the given tweets) according to the evidence. This task is offered in both Arabic and English.

__Table of Content:__

- [List of Versions](#list-of-versions)
- [Contents of Task 5 Directory](#contents-of-task-5-directory)
- [Input Data Format](#input-data-format)
- [Output Data Format](#output-data-format)
- [Evaluation Metrics](#evaluation-metrics)
- [Baselines](#baselines)
- [Scorers](#scorers)
- [Submission runs](#submission-runs)
- [Recommended Reading](#recommended-reading)
- [Task Organizers](#task-organizers)
- [Credits](#credits)

# List of Versions
- **Task 5--Arabic-v1.0 [2024/02/14]** - Arabic Data for Task 5 is released. 
- **Task 5--English-v1.0 [2024/02/14]** - English Data for Task 5 is released. 

# Content of Task 5 Directory

# Input Data Format

We provide train and dev data in JSON format files. Each file contains a list of JSON objects representing rumors. For each rumor, we provide the following entries:
```
{
  id [unique ID for the rumor]
  rumor [rumor tweet text]
  label [the veracity label of the rumor either SUPPORTS, REFUTES, NOT ENOUGH INFO]
  timeline [authorities timeline associated with the rumor each authority tweet is represented by authority Twitter account link, authority tweet ID, authority tweet text]
  evidence [authorities evidence tweets represented by authority Twitter account link, authority tweet ID, authority tweet text]
}
```
**Examples for Arabic data**:

```
{
  "id": "AuRED_089",
  "rumor": "وباء كورونا وصل الى الامارات 75 إصابة في ابوظبي و 63 إصابة في دبي  تحذير للامتناع عن السفر الى الامارات حفاظًا على السلامه و عدم نقل الوباء . اللهم أحفظ المسلمين في كل مكان..." ,
  "label": "REFUTES"
  "timeline": [["https://twitter.com/WHOEMRO", "1222971333522468867", "منظمة الصحة العالمية تعلن فاشية #فيروس_كورونا المستجد طارئة صحة عامة تثير قلقاً دوليا https://t.co/pVOXpZaPH7"],
   ["https://twitter.com/WHOEMRO", "1223608938136047616", "س. هل تحمي اللقاحات المضادة للالتهاب الرئوي من #فيروس_كورونا المستجد؟ ج. لا. لقاحات الالتهاب الرئوي لا تحمي من فيروس كورونا المستجد. هذا الفيروس جديد ومختلف ويحتاج لقاحاً خاصاً به. الباحثون يعملون على تطوير لقاح مضاد لهذا الفيروس. #اعرف_الحقائق https://t.co/QTGmI2flo9"],
   ["https://twitter.com/mohapuae", "1223361274618183681", "تعرف على أعراض فيروس كورونا الجديد #فيروس_كورونا_الجديد #فيروس_كورونا#كورونا#وزارة_الصحة_ووقاية_المجتمع_الإمارات https://t.co/jWALFtA68m"],
   ["https://twitter.com/mohapuae", "1223279618372882432", "مقتطفات من مشاركة وزارة الصحة ووقاية المجتمع في معرض ومؤتمر الصحة العربي2020 من خلال مجموعة من مبادرات ومشاريع الرعاية الصحية المبتكرة تحت شعار "صحة الإمارات مسؤولية مشتركة"#وزارة_الصحة_ووقاية_المجتمع_الإمارات#معرض_ومؤتمر_الصحة_العربي_2020#صحة_الإمارات https://t.co/c69pHj6ffd"],
   ......],
  "evidence": [["https://twitter.com/WHOEMRO","1222506828694794240","أكدت اليوم @WHO ظهور أولى حالات فيروس كورونا المستجد في إقليم شرق المتوسط، بالإمارات العربية المتحدة. عقب تأكيد @mohapuae في 29 يناير.
كان 4 أفراد من نفس العائلة من مدينة ووهان الصينية وصلوا إلى الإمارات في بداية يناير 2020، وتم إدخالهم المستشفى بعد تأكد إصابتهم ب #فيروس_كورونا."],
   ["https://twitter.com/mohapuae", "1222476311291142145", "إصابة أربعة أشخاص من عائلة صينية بفيروس كورونا الجديد جميعهم في حالة مستقرة وتم احتواؤهم وفق الإجراءات الاحترازية المعتمدة عالميا#وزارة_الصحة_ووقاية_المجتمع_الإمارات #فيروس_كورونا_الجديد #فيروس_كورونا https://t.co/ydy2esb20B"]
  ,....]
},
...,
{
  "id": "AuRED_105",
  "rumor": "تونس تعرض مساعدة ليبيا في علاج مصابي انفجار شاحنة الوقود في بنت بيّة #ليبيا #الشاهد للتفاصيل: https://t.co/s7fdU5fvgq" ,
  "label": "SUPPORTS",
  "timeline": [["https://twitter.com/NajlaElmangoush", "1554448728320344064", "أتقدم بالشكر لفخامة رئيس جمهورية #تونس السيد قيس سعيد @TnPresidencyعلى تضامنه وتسخير كل المستشفيات والأطقم الطبية لمساعدة جرحى #بنت_بيه وهذا التضامن يدل على أن ما يجمع الشعبين الشقيقين هو علاقات أخوية وروح تضامنية في السراء والضراء في كل الحالات @OJerandi 🇱🇾🇹🇳"],
  ["https://twitter.com/NajlaElmangoush", "1554027191788355584", "استفاقت بلدية #بنت_بية فجر اليوم على كارثة إنسانية وخبر مفزع، نتيجة انفجار صهريج الوقود، أسفر عن وفاة 5 أشخاص وإصابة قرابة 50 أخرين، أقدم تعازينا الحارة لأهالي المتوفيين، متمنيين الشفاء العاجل للمصابين، اللهم خفف عليهم مصابهم وثبت لهم الآجر."],
  ["https://twitter.com/Mofa_Libya", "1555688484396040193", "ندعوا المجتمع الدولي بالتحرك العاجل والفاعل لوقف التصعيد وتحمل مسؤوليته القانونية والأخلاقية إزاء الشعب الفلسطيني وتوفير الحماية له ، تجدد دولة #ليبيا موقفها الثابت من القضية الفلسطينية والحقوق المشروعة للشعب الفلسطيني الشقيق."],
  ["https://twitter.com/Mofa_Libya", "1555688334533558272", "تعرب وزارة الخارجية والتعاون الدولي بحكومة الوحدة الوطنية عن إدانتها واستنكارها الشديدين للعدوان الإسرائيلي على قطاع غزة مما أسفر عن سقوط شهداء وجرحي بينهم نساء وأطفال. https://t.co/Ijg2BG6F1p"],
......],

"evidence": [["https://twitter.com/Mofa_Libya", "1554448815524139013", "RT @NajlaElmangoush: أتقدم بالشكر لفخامة رئيس جمهورية #تونس السيد قيس سعيد @TnPresidencyعلى تضامنه وتسخير كل المستشفيات والأطقم الطبية لمساعدة جرحى #بنت_بيه وهذا التضامن يدل على أن ما يجمع الشعبين الشقيقين هو علاقات أخوية وروح تضامنية في السراء والضراء في كل الحالات @OJerandi\n 🇱🇾🇹🇳"],
  ["https://twitter.com/Mofa_Libya", "1554446617356427266", "1/2 وزارة الخارجية والتعاون الدولي تعرب عن شكرها وامتنانها العميق لما أعلنت عنه دولة #تونس الشقيقة في بيانها الأخير الذي سخرت فيه مستشفياتها وأطقمها الطبية التونسية؛ لمساعدة الليبيين الذين أصيبوا في بلدية #بنت_بيه إثر إنفجار صهريج الوقود. https://t.co/oWRtH9T7IC"]
....]
},
...
{
  "id": "AuRED_078",
  "rumor": "منظمة الصحة العالمية تدعو لوقف منح الجرعة الثانية من لقاحات كورونا حتى سبتمبر المقبل ما يسمح بايصال الجرعة الاولى من اللقاح للفئات الاكثر " ,
  "label": "NOT ENOUGH INFO",
  "timeline": [["https://twitter.com/DrTedros", "1421857856522002437", "RT @BahraintvNews: الجهود الوطنية للتصدي لفيروس كورونا في مملكة البحرين تبهر المدير العام لمنظمة الصحة العالمية خلال زيارته للمملكة .@WHO @DrTedros  @BDF_Hospital#وزارة_الإعلام  #bahrain  #كورونا_في_البحرين #كورونا  #البحرين #المنامة #صوت_الوطن_وعين_الحدث"],
  ["https://twitter.com/WHOEMRO", "1424064461611147274", "قم بزيارة صفحتنا الجديدة "شركاء في الصحة" عن المملكة العربية #السعودية، الشريك الاستراتيجي القديم ل @WHO وأحد أكبر المانحين، ذات السجل الحافل في دعم المبادرات الصحية العالمية المنقذة للحياة وعمليات الطوارئ.معًا من أجل تحقيق #الصحة_للجميع_وبالجميع https://t.co/0WAwx9mtF1"],
  ["https://twitter.com/WHOEMRO", "1423416531392806920", "❌الادعاء:  ينبغي على كل من تلقى لقاح كوفيد-19 الامتناع عن أخذ أي نوع من أنواع التخدير.✅الحقيقة: في الوقت الحالي، لا توجد أدلة علمية تؤيد أن التخدير يهدد الحياة أو غير آمن للاستخدام بعد تلقي لقاح كوفيد-19.لمزيد من حقائق اللقاح:➡️https://t.co/K7QtTVvBOK https://t.co/eFnCoVF9Jq"],
  ["https://twitter.com/WHOEMRO", "1423261810082426886", "ليس من الأسلم أن تُعطي رضيعك بدائل لبن الأم إذا كنتِ مصابة بمرض #كوفيد_19 إصابةً مؤكدة أو مُشتبهًا فيها 🤱https://t.co/wgp0yMCGnM\n#الأسبوع_العالمي_للرضاعة_الطبيعية https://t.co/B58EIK215r"]
...],

"evidence": []
},

...

```

**Examples for English data**:

```
{
  "id": "AuRED_089",
  "rumor": "The Corona epidemic has reached the Emirates, with 75 cases in Abu Dhabi and 63 cases in Dubai. A warning to refrain from traveling to the Emirates in order to preserve safety and not transmit the epidemic. May Allah protect Muslims everywhere..." ,
  "label": "REFUTES"
  "timeline": [["https://twitter.com/WHOEMRO", "1222971333522468867", "The World Health Organization declares the outbreak of the new #Coronavirus a public health emergency of international concern https://t.co/pVOXpZaPH7"],
   ["https://twitter.com/WHOEMRO", "1223608938136047616", "s. Do pneumonia vaccines protect against the new #Coronavirus? C. no. Pneumonia vaccines do not protect against the new coronavirus. This virus is new and different and needs its own vaccine. Researchers are working to develop a vaccine against this virus. #Know_the_facts https://t.co/QTGmI2flo9"],
   ["https://twitter.com/mohapuae", "1223361274618183681", "Learn about the symptoms of the new Corona virus #NewCoronavirus #Coronavirus #Corona #Ministry of Health and Community Protection https://t.co/jWALFtA68m"],
   ["https://twitter.com/mohapuae", "1223279618372882432", "Excerpts from the participation of the Ministry of Health and Community Protection in the Arab Health Exhibition and Conference 2020 through a group of innovative healthcare initiatives and projects under the slogan “Emirates Health is a Shared Responsibility” #Ministry_of_Health_and_Community_Protection_Emirates #Arab_Health_Exhibition_and_Conference_2020 #Emirates_Health https://t.co/c69pHj6ffd"],
   ......],

"evidence": [["https://twitter.com/WHOEMRO","1222506828694794240","Today @WHO confirmed the emergence of the first cases of the new Coronavirus in the Eastern Mediterranean Region, in the United Arab Emirates. Following @mohapuae's confirmation on January 29. 4 members of the same family from the Chinese city of Wuhan arrived in the UAE at the beginning of January 2020, and were admitted to the hospital after they were confirmed infected with the #Corona_virus."],
   ["https://twitter.com/mohapuae", "1222476311291142145", "Four people from a Chinese family were infected with the new Corona virus, all of whom are in stable condition and were contained according to internationally approved precautionary measures."]
  ,....]
},
...,
{
  "id": "AuRED_105",
  "rumor": "Tunisia offers to help Libya treat those injured in the fuel truck explosion in Bint Biyya #Libya #Witness for details: https://t.co/s7fdU5fvgq" ,
  "label": "SUPPORTS",
  "timeline": [["https://twitter.com/NajlaElmangoush", "1554448728320344064", "I extend my thanks to His Excellency the President of the Republic of #Tunisia, Mr. Kais Saied @TnPresidency, for his solidarity and harnessing all hospitals and medical teams to help the wounded of #BintBey. This solidarity indicates that what unites the two brotherly peoples is fraternal relations and a spirit of solidarity in good times and bad in all cases @OJerandi 🇱🇾🇹🇳"],
  ["https://twitter.com/NajlaElmangoush", "1554027191788355584", "The municipality of #BintBiya woke up at dawn today to a humanitarian catastrophe and terrible news, as a result of a fuel tanker explosion, which resulted in the death of 5 people and the injury of nearly 50 others. I offer our deepest condolences to the families of the deceased, wishing a speedy recovery to the injured. May Allah ease their affliction and grant them reward."],
  ["https://twitter.com/Mofa_Libya", "1555688484396040193", "We call on the international community to take urgent and effective action to stop the escalation and assume its legal and moral responsibility towards the Palestinian people and provide them with protection. The State of #Libya renews its firm position on the Palestinian issue and the legitimate rights of the brotherly Palestinian people."],
  ["https://twitter.com/Mofa_Libya", "1555688334533558272", "The Ministry of Foreign Affairs and International Cooperation of the National Unity Government expresses its strong condemnation and denunciation of the Israeli aggression on the Gaza Strip, which resulted in martyrs and wounded, including women and children. https://t.co/Ijg2BG6F1p"],
......],

"evidence": [["https://twitter.com/Mofa_Libya", "1554448815524139013", "RT @NajlaElmangoush: I extend my thanks to His Excellency the President of the Republic of #Tunisia, Mr. Kais Saied @TnPresidency, for his solidarity and harnessing all hospitals and medical teams to help the wounded of #Bint_Bey. This solidarity indicates that what unites the two brotherly peoples is fraternal relations and a spirit of solidarity in good times and bad in all cases @OJerandi 🇱 🇾🇹🇳"],
  ["https://twitter.com/Mofa_Libya", "1554446617356427266", "1/2 The Ministry of Foreign Affairs and International Cooperation expresses its deep thanks and gratitude for what the sisterly state of #Tunisia announced in its recent statement, in which it made use of its Tunisian hospitals and medical staff. To help Libyans who were injured in the municipality of #Bint_Bey following the fuel tanker explosion. https://t.co/oWRtH9T7IC"]....]
},
...
{
  "id": "AuRED_078",
  "rumor": "The World Health Organization calls for stopping the granting of the second dose of Corona vaccines until next September, which will allow the first dose of the vaccine to be delivered to the groups that need it most in the world." ,
  "label": "NOT ENOUGH INFO",
  "timeline": [["https://twitter.com/DrTedros", "1421857856522002437", "RT @BahraintvNews: The national efforts to combat the Corona virus in the Kingdom of Bahrain impress the Director-General of the World Health Organization during his visit to the Kingdom. . . @WHO @DrTedros @BDF_Hospital . . #Ministry_of_Information #bahrain #Corona_in_Bahrain #Corona #Bahrain #Manama #Voice_of_the_Nation_and_Ain_Hadath"],
  ["https://twitter.com/WHOEMRO", "1424064461611147274", "Visit our new “Partners in Health” page about #SaudiArabia, a long-standing strategic partner of @WHO and one of our largest donors, with a proven track record of supporting life-saving global health initiatives and emergency operations. Together for #HealthForAll, By All https:/ /t.co/0WAwx9mtF1\""],
  ["https://twitter.com/WHOEMRO", "1423416531392806920", "❌Claim: Anyone who has received the Covid-19 vaccine should refrain from taking any type of anesthesia. ✅Fact: Currently, there is no scientific evidence to support that anesthesia is life-threatening or unsafe to use after receiving the COVID-19 vaccine. For more vaccine facts: ➡️https://t.co/K7QtTVvBOK https://t.co/eFnCoVF9Jq"],
  ["https://twitter.com/WHOEMRO", "1423261810082426886", "It is not safe to give your baby breastmilk substitutes if you have a confirmed or suspected case of #Covid_19 🤱https://t.co/wgp0yMCGnM #WorldBreastfeedingWeek https://t.co/B58EIK215r"]
....],

"evidence": []
},

...

```

# Output Data Format

We provide examples of output data format expected from the participants. Each submission should **include both** the evidence retrieval and the rumor verification output in the below format [sample examples](https://gitlab.com/checkthat_lab/clef2024-checkthat-lab/-/blob/main/task5/submission_samples) are provided):

**Evidence retrieval output format** (For each rumor a **maximum of 5 evidence authority tweets** should be retrieved) 

Each row of the result file is related to a pair (rumor_id, authority_tweet_id) and intuitively indicates the ranking of the authorities evidence with respect to the input rumor.
Each row should be in the following format:

> rumor_id <TAB> Q0 <TAB> authority_tweet_id <TAB> rank <TAB> score <TAB> tag

where <br/>
* rumor_id: The unique ID for the given rumor
* Q0: This column is needed to comply with the TREC format
* authority_tweet_id: The unique ID for the authority tweet ID
* rank: the rank of the authority tweet ID for that given rumor_id
* score: the score given by your model for the authority tweet ID given the rumor
* tag: the string identifier of the team/model

**Rumor Verification output format**

The file should be in json format. Each entry should be in the following format:
 > {"id": rumor_id,"predicted_label": rumor label as predicted by your model,
  "predicted_evidence":[[authority, authority tweet ID, authority tweet text, score as predicted by your model], 
                        [authority, authority tweet ID, authority tweet text, score as predicted by your model],
                        [authority, authority tweet ID, authority tweet text, score as predicted by your model],
                        ...]}

# Evaluation Metrics

The official evaluation measure for evidence retrieval is Mean Average Precision (MAP). The systems get no credit if they retrieve any tweets for unverifiable rumors. Other evaluation measures to be considered are Recall@5.

We use the Macro-F1 to evaluate the classification of the rumors. Additionally, we will consider a Strict Macro-F1 where the rumor label is considered correct only if at least one retrieved authority evidence is correct.

# baselines
Our baseline is [KGAT](https://github.com/thunlp/KernelGAT) originally proposed for fact checking using evidence from Wikipedia pages. .We fine-tuned KGAT using the authors’ setup for both evidence retrieval and claim verification tasks. As our baseline we consider a zero-shot setup where we fine-tune the model using the English data (FEVER) and test on our task data. Differently, we replaced the English BERT with multilingual BERT (mBERT) to be able to test on both Arabic and English data for our task.

**Zero-shot KGAT evidence retrieval results on dev sets**
|Test set|MAP|R@5|
|:----|:----|:----|
|Arabic dev set|0.595|0.645|
|English dev set|0.561|0.636|

**Zero-shot KGAT rumor verification results on dev sets**
|Test set|Macro-F1|Strict Macro-F1|
|:----|:----|:----|
|Arabic dev set|0.454|0.410|
|English dev set|0.508|0.508|

# Scorers

To evaluate your models, we provide a [evidence retrieval scorer](https://gitlab.com/checkthat_lab/clef2024-checkthat-lab/-/blob/main/task5/scorer/retrieval_scorer.py), and [rumor verification scorer](https://gitlab.com/checkthat_lab/clef2024-checkthat-lab/-/blob/main/task5/scorer/verification_scorer.py) 

To run the scorer you need to install [Pyterrier](https://pyterrier.readthedocs.io/en/latest/)

To evaluate your evidence retrieval model. Please run the following:  

> python pyterrier_scorer.py --pred_file submission_samples/KGAT_zeroShot_evidence_Arabic_dev.txt \
      --actual_file data/dev_qrels.txt \
      --output_file KGAT_zeroShot_retrieval_Arabic_dev.csv
 <br/>

To evaluate your rumor verification model. Please run the following:

> python verification_scorer.py \
 --actual_file data/Arabic_dev.json \
 --pred_file submission_samples/KGAT_zeroShot_verification_Arabic_dev.json \
 --out_file  KGAT_zeroShot_dev_fold5_results.cs
<br/>

# Submission runs

Each team can submit up to **3 runs per language** where:

* For each run per language, you will have to explicitly indicate if it is **primary** or **secondary**. 
* **One and only one** run per language **must** be **primary** and it will be used for main comparison to other systems.
* For each run, you will have to explicitly indicate if external data was used for training. 
* Maximum of **one run per language** can **use external data**.

# Recommended Reading

The following papers might be useful for the task. We have not provided an exhaustive list, but this could be a good start.

* Fatima Haouari, Tamer Elsayed, and Watheq Mansour. 2023. Who can verify this? Finding authorities for rumor verification in Twitter. Information Processing & Management 60, 4 (2023), 103366.
* Fatima Haouari and Tamer Elsayed. 2023. Detecting Stance of Authorities Towards Rumors in Arabic Tweets: A Preliminary Study. In Advances in Information Retrieval. Springer Nature Switzerland, Cham, 430–438.
* Fatima Haouari and Tamer Elsayed. 2024. Are authorities denying or supporting? Detecting stance of authorities towards rumors in Twitter. Social Network Analysis and Mining 14, 1 (2024), 34.
* James Thorne, Andreas Vlachos, Oana Cocarascu, Christos Christodoulopoulos, and Arpit Mittal. 2018. The Fact Extraction and VERification (FEVER) Shared Task. In Proceedings of the First Workshop on Fact Extraction and VERification (FEVER),
James Thorne, Andreas Vlachos, Oana Cocarascu, Christos Christodoulopoulos, and Arpit Mittal (Eds.). Association for Computational Linguistics, Brussels, Belgium, 1–9. https://doi.org/10.18653/v1/W18-5501
* Zhenghao Liu, Chenyan Xiong, Maosong Sun, and Zhiyuan Liu. 2020. Fine-grained Fact Verification with Kernel Graph Attention Network. In Proceedings of the 58th Annual Meeting of the Association for Computational Linguistics. 7342–7351.
* Giannis Bekoulis, Christina Papagiannopoulou, and Nikos Deligiannis. 2021. Understanding the Impact of Evidence-Aware Sentence Selection for Fact Checking. In Proceedings of the Fourth Workshop on NLP for Internet Freedom: Censorship,
Disinformation, and Propaganda, Anna Feldman, Giovanni Da San Martino, Chris Leberknight, and Preslav Nakov (Eds.). Association for Computational Linguistics, Online, 23–28. https://doi.org/10.18653/v1/2021.nlp4if-1.4
* Canasai Kruengkrai, Junichi Yamagishi, and Xin Wang. 2021. A Multi-Level Attention Model for Evidence-Based Fact Checking. In Findings of the Association for Computational Linguistics: ACL-IJCNLP 2021, Chengqing Zong, Fei Xia, Wenjie
Li, and Roberto Navigli (Eds.). Association for Computational Linguistics, Online, 2447–2460. https://doi.org/10.18653/v1/2021.findings-acl.217


# Task Organizers
- [Tamer Elsayed](http://qufaculty.qu.edu.qa/telsayed/) (Qatar University)
- [Fatima Haouari](https://sites.google.com/view/bigir/members/fatima-haouari) (Qatar University)
- [Reem Suwaileh](https://sites.google.com/view/bigir/members/reem-suwaileh) (HBKU, Qatar)

# Credits
Please find it on the task website: https://checkthat.gitlab.io/clef2024/task5/
