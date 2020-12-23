### Entry format

Each entry has following fields:

 * `uid`: The unique identifier of the question, an integer. Note that identifiers are not consecutive.
 * `question_text`: The original question text in Russian.
 * `question_eng`: The machine translation of the question in English.
 * `answer_text`: The answer text as given in source of the questions. Not always the same as textual representation of golden answers.
 * `query`: The corresponding SPARQL query. It is guaranteed to produce the golden answer when executed on the provided Wikidata sample.
 * `question_uris`: An array of the Wikidata URIs of entities used in the SPARQL query.
 * `question_props`: An array of the Wikidata properties used in the SPARQL query.
 * `answers`: An array of golden answers for the question. Each answer has at least two following fields:
   * `type`: Type of the answer. It's value can be either `uri` or `literal`.
   * `value`: The answer content. It is a entry URI if type is `uri` and a raw string if type is `literal`.
 <br> If type is `literal`, additional fields like `xml:lang` or `datatype` may be presented.
 * `tags`: An array of tags describing query structure. See detailed tags description below.

### Query tags

This table shows meaning of query tags in RuBQ and their distribution across test and development subsets.

| Tag | #Dev/Test | Description |
| --- | --- | --- |
| 1-hop | 198/760 | Query corresponds to a single SPO triple. | 
| multi-hop | 14/55 | Query's constraint is applied to more than one fact. | 
| multi-constraint | 21/110 | Query contains more than one SPARQL constraint. |
| qualifier-answer | 1/5 | Answer is a value of a qualifier relation, similar to *fact with qualifiers* in LC-QuAD 2.0. |
| qualifier-constraint | 4/22 | Query poses constraints on qualifier relations; a superclass of *temporal aspect* in LC-QuAD 2.0. |
| reverse | 6/29 | Answer's variable is a subject in at least one constraint. |
| count | 1/4 | Query applies COUNT operator to the resulting entities, same as in LC-QuAD 2.0. |
| ranking | 3/16 | ORDER and LIMIT operators are applied to the entities specified by constraints, same as in LC-QuAD 2.0. |
| 0-hop | 3/12 | Query returns an entity already mentioned in the questions. The corresponding questions usually contain definitions or entity's alternative names. | 
| exclusion | 4/18 | Query contains NOT IN, which typically excludes entities mentioned in the question from the answer. |
| no\_answer | 60/240 | Question cannot be answered with the knowledge base, although answer entity may be present in the KB. |
