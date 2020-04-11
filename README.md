# RuBQ: A Russian Knowledge Base Question Answering Data Set

## Introduction

We present **RuBQ** (pronounced \[\`rubik\]) -- **Ru**ssian Knowledge **B**ase **Q**uestions, a KBQA dataset that consists of 1,500 Russian questions of varying complexity along with their English machine translations, corresponding SPARQL queries, answers, as well as a subset of Wikidata covering entities with Russian labels. To the best of our knowledge, this is the first Russian KBQA and semantic parsing dataset. The dataset is thought to be used as a development and test sets in cross-lingual transfer, few-shot learning, or  learning with synthetic data scenarios. 

## Download

## Usage

### Format

### Examples

| Question | Query | Answers | Tags |
| :--- | :--- | :--- | :---- |
| **Rus**: Кто написал роман «Хижина дяди Тома»? <br><br> **Eng**: Who wrote the novel "Uncle Tom's Cabin"? | <pre>SELECT ?answer <br>WHERE {<br>  wd:Q2222 wdt:P50 ?answer .<br>}</pre> | wd:Q102513 <br> (Harriet Beecher Stowe) | 1-hop |
| **Rus**: Кто сыграл князя Андрея Болконского в фильме С. Ф. Бондарчука «Война и мир»? <br><br> **Eng**: Who played Prince Andrei Bolkonsky in S. F. Bondarchuk's film "War and peace"? | <pre>SELECT ?answer<br>WHERE {<br>  wd:Q845176 p:P161 [<br>    ps:P161 ?answer; <br>    pq:P453 wd:Q2737140<br>  ] .<br>}</pre> | wd:Q312483 <br> (Vyacheslav Tikhonov) | qualifier-constraint |
| **Rus**: Кто на работе пользуется теодолитом? <br><br> **Eng**: Who uses a theodolite for work? | <pre>SELECT ?answer <br>WHERE {<br>  wd:Q181517 wdt:P366 [<br>    wdt:P3095 ?answer<br>  ] .<br>}</pre> | wd:Q1734662<br> (cartographer)  <br> wd:Q11699606<br> (geodesist) <br> wd:Q294126<br> (land surveyor)  | multi-hop |
| **Rus**: Какой океан самый маленький? <br><br> **Eng**: Which ocean is the smallest? | <pre>SELECT ?answer <br>WHERE {<br>  ?answer p:P2046/<br>     psn:P2046/<br>     wikibase:quantityAmount ?sq .<br>  ?answer wdt:P31 wd:Q9430 .<br>}<br>ORDER BY ASC(?sq)<br>LIMIT 1</pre> | wd:Q788<br>(Arctic Ocean) | multi-constraint<br><br>reverse<br><br>ranking |

### RuWikidata Sample

### *rdfs:label* and *skos:altLabel* predicates convention

## Evaluation

## Leaderboard

This work is licensed under a [Creative Commons Attribution 4.0 International License][cc-by].

[![CC BY 4.0][cc-by-image]][cc-by]

[cc-by]: http://creativecommons.org/licenses/by/4.0/
[cc-by-image]: https://i.creativecommons.org/l/by/4.0/88x31.png
[cc-by-shield]: https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg
