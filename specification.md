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
