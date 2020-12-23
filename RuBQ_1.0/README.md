# RuBQ: A Russian Knowledge Base Question Answering Data Set

## Introduction

We present **RuBQ** (pronounced \[\`rubik\]) -- **Ru**ssian Knowledge **B**ase **Q**uestions, a KBQA dataset that consists of 1,500 Russian questions of varying complexity along with their English machine translations, corresponding SPARQL queries, answers, as well as a subset of Wikidata covering entities with Russian labels. 300 RuBQ questions are unanswerable, which poses a new challenge for KBQA systems and makes the task more realistic. The dataset is based on a collection of quiz questions. The data generation pipeline combines automatic processing, crowdsourced and in-house verification, see details in the paper. To the best of our knowledge, this is the first Russian KBQA and semantic parsing dataset. 

## Links

[Latest ISWC 2020 paper](https://link.springer.com/chapter/10.1007/978-3-030-62466-8_7) :page_facing_up:

[Older paper on arXiv](https://arxiv.org/abs/2005.10659) :page_facing_up:

[Test](RuBQ_test.json) and [Dev](RuBQ_dev.json) subsets

[RuWikidata](http://doi.org/10.5281/zenodo.3751761) sample

Dataset is also published on [Zenodo](http://doi.org/10.5281/zenodo.3835913)

## Usage

The dataset is thought to be used as a development and test sets in cross-lingual transfer, few-shot learning, or learning with synthetic data scenarios.

### Format

Data set files are presented in JSON format as an array of dictionary entries. See full specifications [here](specification.md).

### Examples

| Question | Query | Answers | Tags |
| :--- | :--- | :--- | :---- |
| **Rus**: Кто написал роман «Хижина дяди Тома»? <br><br> **Eng**: Who wrote the novel "Uncle Tom's Cabin"? | <pre>SELECT ?answer <br>WHERE {<br>  wd:Q2222 wdt:P50 ?answer .<br>}</pre> | wd:Q102513 <br> (Harriet Beecher Stowe) | 1-hop |
| **Rus**: Кто сыграл князя Андрея Болконского в фильме С. Ф. Бондарчука «Война и мир»? <br><br> **Eng**: Who played Prince Andrei Bolkonsky in S. F. Bondarchuk's film "War and peace"? | <pre>SELECT ?answer<br>WHERE {<br>  wd:Q845176 p:P161 [<br>    ps:P161 ?answer; <br>    pq:P453 wd:Q2737140<br>  ] .<br>}</pre> | wd:Q312483 <br> (Vyacheslav Tikhonov) | qualifier-constraint |
| **Rus**: Кто на работе пользуется теодолитом? <br><br> **Eng**: Who uses a theodolite for work? | <pre>SELECT ?answer <br>WHERE {<br>  wd:Q181517 wdt:P366 [<br>    wdt:P3095 ?answer<br>  ] .<br>}</pre> | wd:Q1734662<br> (cartographer)  <br> wd:Q11699606<br> (geodesist) <br> wd:Q294126<br> (land surveyor)  | multi-hop |
| **Rus**: Какой океан самый маленький? <br><br> **Eng**: Which ocean is the smallest? | <pre>SELECT ?answer <br>WHERE {<br>  ?answer p:P2046/<br>     psn:P2046/<br>     wikibase:quantityAmount ?sq .<br>  ?answer wdt:P31 wd:Q9430 .<br>}<br>ORDER BY ASC(?sq)<br>LIMIT 1</pre> | wd:Q788<br>(Arctic Ocean) | multi-constraint<br><br>reverse<br><br>ranking |

### RuWikidata8M Sample

We provide a Wikidata sample containing all the entities with Russian labels. It consists of about 212M triples with 8.1M unique entities. This snapshot mitigates the problem of Wikidata’s dynamics – a reference answer may change with time as the knowledge base evolves. The sample guarantees the correctness of the queries and answers. In addition, the smaller dump makes it easier to conduct experiments with our dataset.

We strongly recommend using this sample for evaluation.

#### Details

Sample is a collection of several RDF files in Turtle.

 - `wdt_all.ttl` contains all the truthy statements.
 - `names.ttl` contains Russian and English labels and aliases for all entities. Names in other language also provided when needed.
 - `onto.ttl` contains all Wikidata triples with relation `wdt:P279` - *subclass of*. It represents some class hierarchy, but remember that there is no *class* or *instance* concepts in Wikidata.
 - `pch_{0,6}.ttl` contain all statetment nodes and their data for all entities.

## Evaluation

### *rdfs:label* and *skos:altLabel* predicates convention

Some question in our dataset require using *rdfs:label* or *skos:altLabel* for retrieving answer which is a literal. In cases where answer language doesn't have to be inferred from question, our evaluation script takes into account Russian literals only.

## Reference

If you use RuBQ dataset in your work, please cite:

```
@article{korablinov2020rubq,
  title={RuBQ: A Russian Dataset for Question Answering over Wikidata},
  author={Vladislav Korablinov and Pavel Braslavski},
  journal={arXiv preprint arXiv:2005.10659},
  year={2020}
}
```

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0
International License][cc-by-sa].

[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa]

[cc-by-sa]: http://creativecommons.org/licenses/by-sa/4.0/
[cc-by-sa-image]: https://licensebuttons.net/l/by-sa/4.0/88x31.png
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg
