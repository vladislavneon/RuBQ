# RuBQ 2.0: An Innovated Russian Question Answering Dataset

## Introduction

We present the second version of RuBQ. The dataset extension is based on questions obtained through search engine query suggestion services. The dataset doubled in size: **RuBQ 2.0** contains 2,910 questions along with the answers and SPARQL queries. Starting with limited manual data preparation, we carried out most of the annotation using crowdsourcing and automated routines. We expanded the dataset with machine reading comprehension capabilities: RuBQ 2.0 incorporates answer-bearing paragraphs from Wikipedia for the majority of questions. Thus, the dataset is now not only suitable for the evaluation of KBQA, but also can be used to evaluate machine reading comprehension, paragraph retrieval, and end-to-end open-domain question answering. The dataset can be also used for experiments in hybrid QA, where KBQA and text-based QA can enrich and complement each other.

## Links

[ESWC 2020 paper](https://link.springer.com/chapter/10.1007/978-3-030-77385-4_32) :page_facing_up:

[Paper discussion at OpenReview](https://openreview.net/forum?id=P5UQFFoQ4PJ) :page_facing_up:

[Test](RuBQ_2.0_test.json) and [Dev](RuBQ_2.0_dev.json) subsets

[Related paragraphs](RuBQ_2.0_paragraphs.json)

[RuWikidata](http://doi.org/10.5281/zenodo.3751761) sample

Dataset is also published on [Zenodo](https://doi.org/10.5281/zenodo.4345696)

## Usage

RuBQ 2.0 is suitable for the evaluation of KBQA, MRC, paragrph retrieval, and end-to-end open-domain question answering. 
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
| **Rus**: Сколько дней продолжалась Курская битва? <br><br> **Eng**: How many days did the battle of Kursk last? | <pre>SELECT ?answer <br>WHERE {<br>  wd:Q130861 wdt:P580 ?begin . <br>     wd:Q130861 wdt:P582 ?end .<br>  BIND (xsd:integer(?end - ?begin + 1) AS ?answer).<br>}</pre> | 50 | duration |

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

## Reference

If you use RuBQ dataset in your work, please cite:

```
@inproceedings{RuBQ2021,
  title={{RuBQ} 2.0: An Innovated {Russian} Question Answering Dataset},
  author={Ivan Rybin and Vladislav Korablinov and Pavel Efimov and Pavel Braslavski},
  booktitle={ESWC},
  year={2021},
  pages={532--547}
}
```

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0
International License][cc-by-sa].

[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa]

[cc-by-sa]: http://creativecommons.org/licenses/by-sa/4.0/
[cc-by-sa-image]: https://licensebuttons.net/l/by-sa/4.0/88x31.png
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg
