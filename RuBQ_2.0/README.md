# RuBQ 2.0: An Innovated Russian Question Answering Dataset

## Introduction

We present the second version of RuBQ. The dataset extension is based on questions obtained through search engine query suggestion services. The dataset doubled in size: **RuBQ 2.0** contains 2,910 questions along with the answers and SPARQL queries. Starting with limited manual data preparation, we carried out most of the annotation using crowdsourcing and automated routines. We expanded the dataset with machine reading comprehension capabilities: RuBQ 2.0 incorporates answer-bearing paragraphs from Wikipedia for the majority of questions. Thus, the dataset is now not only suitable for the evaluation of KBQA, but also can be used to evaluate machine reading comprehension, paragraph retrieval, and end-to-end open-domain question answering. The dataset can be also used for experiments in hybrid QA, where KBQA and text-based QA can enrich and complement each other.

## Links

[Paper discussion at OpenReview](https://openreview.net/forum?id=P5UQFFoQ4PJ) :page_facing_up:

[Test](RuBQ_2.0_test.json) and [Dev](RuBQ_2.0_dev.json) subsets

[Related paragraphs](RuBQ_2.0_paragraphs.json)

[RuWikidata](http://doi.org/10.5281/zenodo.3751761) sample

Dataset is also published on [Zenodo](https://doi.org/10.5281/zenodo.4345696)

## Usage

RuBQ 2.0 is suitable for the evaluation of KBQA, MRC, paragraph retrieval, and end-to-end open-domain question answering. 
The dataset is thought to be used primarily for testing rule-based systems, models based on few/zero-shot and transfer learning, as well as models trained on automatically generated examples, similarly to recent MRC datasets. One also can use RuBQ 2.0 as a development and test sets in cross-lingual transfer, few-shot learning, or learning with synthetic data scenarios.

### Format

Data set files are presented in JSON format as an array of dictionary entries. See full specifications [here](specification_v2.0.md).

### Examples

Inherited from RuBQ 1.0:

| Question | Query | Answers | Tags |
| :--- | :--- | :--- | :---- |
| **Rus**: Кто написал роман «Хижина дяди Тома»? <br><br> **Eng**: Who wrote the novel "Uncle Tom's Cabin"? | <pre>SELECT ?answer <br>WHERE {<br>  wd:Q2222 wdt:P50 ?answer .<br>}</pre> | wd:Q102513 <br> (Harriet Beecher Stowe) | 1-hop |
| **Rus**: Кто сыграл князя Андрея Болконского в фильме С. Ф. Бондарчука «Война и мир»? <br><br> **Eng**: Who played Prince Andrei Bolkonsky in S. F. Bondarchuk's film "War and peace"? | <pre>SELECT ?answer<br>WHERE {<br>  wd:Q845176 p:P161 [<br>    ps:P161 ?answer; <br>    pq:P453 wd:Q2737140<br>  ] .<br>}</pre> | wd:Q312483 <br> (Vyacheslav Tikhonov) | qualifier-constraint |
| **Rus**: Кто на работе пользуется теодолитом? <br><br> **Eng**: Who uses a theodolite for work? | <pre>SELECT ?answer <br>WHERE {<br>  wd:Q181517 wdt:P366 [<br>    wdt:P3095 ?answer<br>  ] .<br>}</pre> | wd:Q1734662<br> (cartographer)  <br> wd:Q11699606<br> (geodesist) <br> wd:Q294126<br> (land surveyor)  | multi-hop |
| **Rus**: Какой океан самый маленький? <br><br> **Eng**: Which ocean is the smallest? | <pre>SELECT ?answer <br>WHERE {<br>  ?answer p:P2046/<br>     psn:P2046/<br>     wikibase:quantityAmount ?sq .<br>  ?answer wdt:P31 wd:Q9430 .<br>}<br>ORDER BY ASC(?sq)<br>LIMIT 1</pre> | wd:Q788<br>(Arctic Ocean) | multi-constraint<br><br>reverse<br><br>ranking |
| **Rus**: Сколько дней продолжалась Курская битва? <br><br> **Eng**: How many days did the battle of Kursk last? | <pre>SELECT ?answer <br>WHERE {<br>  wd:Q130861 wdt:P580 ?begin . <br>  wd:Q130861 wdt:P582 ?end .<br>  BIND (xsd:integer(?end - ?begin + 1) AS ?answer).<br>}</pre> | 50 | duration |

New in RuBQ 2.0, answer names:
(lists of Wikidata names may be truncated)

| Question | Answers | WD Label | WD Names | WP Names
| :--- | :--- | :--- | :----- | :--- |
| **Rus**: Кто написал роман «Хижина дяди Тома»? <br><br> **Eng**: Who wrote the novel "Uncle Tom's Cabin"? |  wd:Q102513 | Гарриет Бичер-Стоу | **Ru**: Стоу Гарриет Бичер,<br>Бичер-Стоу Гарриет,<br>Гарриет Бичер-Стоу,<br>Бичер Стоу,<br>...<br> **En**: Christopher Crowfield,<br>Harriet Elizabeth Beecher Stowe,<br>Enrieta Elizabeth Beecher Stowe,<br>Harriet Beecher Stowe | Гарриет Бичер-Стоу |
| **Rus**: Кто сыграл князя Андрея Болконского в фильме С. Ф. Бондарчука «Война и мир»? <br><br> **Eng**: Who played Prince Andrei Bolkonsky in S. F. Bondarchuk's film "War and peace"? | wd:Q312483 | Вячеслав Васильевич Тихонов | **Ru**: Тихонов, Вячеслав,<br>Вячеслав Тихонов,<br>Тихонов Вячеслав Васильевич,<br>Вячеслав Васильевич Тихонов,<br>...<br> **En**: Vyacheslav Tikhonov | Вячеслав Васильевич Тихонов |
| **Rus**: Кто на работе пользуется теодолитом? <br><br> **Eng**: Who uses a theodolite for work? | wd:Q1734662<br><br><br><br> wd:Q11699606<br><br><br> wd:Q294126<br><br><br> | картограф<br><br><br><br>инженер-геодезист<br><br><br>землемер<br><br><br> | **Ru**: картограф<br> **En**: map maker,<br>mapmaker,<br>cartographer<br> **Ru**: инженер-геодезист<br> **En**: geodesist<br> **Ru**: землемер<br> **En**: surveyor,<br>land surveyor | - |
| **Rus**: Какой океан самый маленький? <br><br> **Eng**: Which ocean is the smallest? | wd:Q788 | Северный Ледовитый океан | **Ru**: Северный Ледовитый океан<br> **En**: Northern Ocean,<br>Arctic Ocean,<br>Arctic Sea | Северного Ледовитого океана |
| **Rus**: Сколько дней продолжалась Курская битва? <br><br> **Eng**: How many days did the battle of Kursk last? | 50 | 50 | **Ru**: -<br> **En**: -| - |

New in RuBQ 2.0, paragraphs:
(lists of all related paragraphs IDs are truncated)

| Question | Paragraph IDs | The paragraph <br>with answer | A paragraph <br>without answer |
| :----- | :--- | :--- | :--- |
| **Rus**: Кто написал роман «Хижина дяди Тома»? <br><br> **Eng**: Who wrote the novel "Uncle Tom's Cabin"? | **With answer**: 35652<br> **All related**: 35652,<br>35653,<br>35654,<br>35655,<br>35656,<br>... | «Хижина дяди Тома» (англ. Uncle Tom's Cabin) — роман Гарриет Бичер-Стоу 1852 года, направленный против рабовладения в Америке. Роман произвёл большой общественный резонанс; по некоторым оценкам, настолько обострил один из местных конфликтов на почве рабства, что он вылился в Гражданскую войну в США. | В ответ на обвинения Бичер-Стоу в 1853 году написала книгу «Ключ к хижине дяди Тома», в которой приводились свидетельства того, что роман основан на реальных событиях. Прототипом главного героя романа послужил беглый негр-раб Джосайя Хенсон. |
| **Rus**: Кто сыграл князя Андрея Болконского в фильме С. Ф. Бондарчука «Война и мир»? <br><br> **Eng**: Who played Prince Andrei Bolkonsky in S. F. Bondarchuk's film "War and peace"? | **With answer**: 30501<br> **All related**: 30501,<br>30504,<br>30505,<br>30506,<br>30507,<br>... | «Война и мир» (1965, СССР). Реж. — С. Бондарчук, в главных ролях: Наташа Ростова — Людмила Савельева, Андрей Болконский — Вячеслав Тихонов, Пьер Безухов — Сергей Бондарчук. | Анатоль (Анатолий Васильевич) Курагин — младший сын князя Василия. |
| **Rus**: Кто на работе пользуется теодолитом? <br><br> **Eng**: Who uses a theodolite for work? | **With answer**: -<br> **All related**: 47679,<br>47680,<br>47681,<br>47682,<br>47683,<br>... | - | Ось цилиндрического уровня при алидаде горизонтального круга должна быть перпендикулярна к оси вращения алидады. |
| **Rus**: Какой океан самый маленький? <br><br> **Eng**: Which ocean is the smallest? | **With answer**: 37852<br> **All related**: 3596,<br>3597,<br>3598,<br>51121,<br>51122,<br>... | Северный Ледовитый океан (англ. Arctic Ocean, дат. Ishavet, норв. и нюнорск Nordishavet) — наименьший по площади океан Земли, расположен между Евразией и Северной Америкой. Площадь 14,75 млн км², то есть чуть больше 4 % от всей площади Мирового океана, объём воды 18,07 млн км³. Северный Ледовитый океан самый мелководный из всех океанов, его средняя глубина составляет 1225 м (наибольшая глубина 5527 м в Гренландском море). | Большую часть рельефа дна Северного Ледовитого океана занимает шельф (более 45 % дна океана) и подводные окраины материков (до 70 % площади дна). Именно этим объясняется малая средняя глубина океана — около 40 % его площади имеет глубины меньше 200 м. ... |
| **Rus**: Сколько дней продолжалась Курская битва? <br><br> **Eng**: How many days did the battle of Kursk last? | **With answer**: 56951<br> **All related**: 56951,<br>30210,<br>30211,<br>30212,<br>30213,<br>... | Сражение является важнейшей частью стратегического плана летне-осенней кампании 1943 года, согласно советской и российской историографии, включает в себя: Курскую стратегическую оборонительную операцию (5—23 июля), Орловскую (12 июля — 18 августа) и Белгородско-Харьковскую (3—23 августа) стратегические наступательные операции. Битва продолжалась 50 дней. Немецкая сторона наступательную часть сражения называла операция «Цитадель». | Важной составляющей успеха Красной Армии стала совокупность мероприятий по военно-экономическому обеспечению войск. |

### RuWikidata8M Sample

We provide a Wikidata sample containing all the entities with Russian labels. It consists of about 212M triples with 8.1M unique entities. This snapshot mitigates the problem of Wikidata’s dynamics – a reference answer may change with time as the knowledge base evolves. The sample guarantees the correctness of the queries and answers. In addition, the smaller dump makes it easier to conduct experiments with our dataset.

We strongly recommend using this sample for evaluation.

#### Details

Sample is a collection of several RDF files in Turtle.

 - `wdt_all.ttl` contains all the truthy statements.
 - `names.ttl` contains Russian and English labels and aliases for all entities. Names in other language also provided when needed.
 - `onto.ttl` contains all Wikidata triples with relation `wdt:P279` - *subclass of*. It represents some class hierarchy, but remember that there is no *class* or *instance* concepts in Wikidata.
 - `pch_{0,6}.ttl` contain all statetment nodes and their data for all entities.

<!-- ## Evaluation

### *rdfs:label* and *skos:altLabel* predicates convention

Some question in our dataset require using *rdfs:label* or *skos:altLabel* for retrieving answer which is a literal. In cases where answer language doesn't have to be inferred from question, our evaluation script takes into account Russian literals only. -->

<!-- ## Reference

If you use RuBQ dataset in your work, please cite:

```
@article{korablinov2020rubq,
  title={RuBQ: A Russian Dataset for Question Answering over Wikidata},
  author={Vladislav Korablinov and Pavel Braslavski},
  journal={arXiv preprint arXiv:2005.10659},
  year={2020}
}
``` -->

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0
International License][cc-by-sa].

[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa]

[cc-by-sa]: http://creativecommons.org/licenses/by-sa/4.0/
[cc-by-sa-image]: https://licensebuttons.net/l/by-sa/4.0/88x31.png
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg
