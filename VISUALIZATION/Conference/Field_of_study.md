##  Field of Study

A single paper in MAG is tagged with multiple fields so we count that paper into multiple fields.
Example: <a href="https://academic.microsoft.com/paper/2618530766/reference/search?q=ImageNet%20classification%20with%20deep%20convolutional%20neural%20networks&qe=Or(Id%253D2194775991%252CId%253D2097117768%252CId%253D2911964244%252CId%253D2108598243%252CId%253D1904365287%252CId%253D1665214252%252CId%253D2546302380%252CId%253D2110764733%252CId%253D2766736793%252CId%253D2130325614%252CId%253D1576445103%252CId%253D2166049352%252CId%253D2141125852%252CId%253D2108069432%252CId%253D2134557905%252CId%253D2053229256%252CId%253D2156163116%252CId%253D2015861736%252CId%253D2026942141%252CId%253D1499991161%252CId%253D2169805405%252CId%253D2396976214%252CId%253D2097356275%252CId%253D2951128674%252CId%253D2144161366%252CId%253D2061212083%252CId%253D2154579312%252CId%253D2101926813%252CId%253D1573503290%252CId%253D2565808444%252CId%253D2018435387)&f=&orderBy=0">Imagenet</a>
paper will be tagged with computer science, Machine learning, artficial intelligence, convolutional neural networks. So we count this paper in each field of study.


### MAG Concepts
- MAG uses concepts to categorize the fields of study of publications.
- Concepts are abstract and hence have no concrete way to define them.
- Concepts are hierarchical. For example, “machine learning” is a concept frequently associated with “artificial intelligence” that, in turn, is a branch of “computer science” but often intersects with “cognitive science” in “psychology.”
- MAG uses a state of the art semantic understanding algorithms to recognize and organize concepts in each publication.( https://www.frontiersin.org/articles/10.3389/fdata.2019.00045/full#F1  )
- The algorithm relies on a core capability to quantify the semantic distance between two textual paragraphs.
- With this semantic distance measure, passages that are addressing a similar topic are clustered together and their joint semantics forms the basis of a concept.
- Assumption principle is introduced (Sinha et al., 2015: https://dl.acm.org/doi/pdf/10.1145/2740908.2742839 ) to organize the concepts into the hierarchical structure as currently in MAG. Simply put, if a concept is always present in the context of another, MAG treats the former as a child concept of the latter.
- There is no mathematical guarantee that the hierarchy thus formed is unique.
- MAG manually defines the top two levels of concepts where various categorization systems seem to agree most. The rest of the six levels are left to the algorithm to decide organically. https://www.mitpressjournals.org/doi/full/10.1162/qss_a_00021
- Although new concepts are detected dynamically as they are observed in the documents, the hierarchy is only recomputed every six months to minimize the instabilities of the data sets.
- Internally, each concept is a collection of semantic representations of textual passages, and these abstract representations, though not readable to humans, are adequate for machine reasoning, such as finding the “is related to” relations among concepts or publications in MAG( https://dl.acm.org/doi/pdf/10.1145/3308558.3313700 ).
- Concept Detection and Taxonomy Learning: ( https://www.frontiersin.org/articles/10.3389/fdata.2019.00045/full#F1  )
- A taxonomy must allow a concept to have multiple parents and organize all concepts into a directed acyclic graph (DAG). 
- While concepts can be associated with all types of physical entities, say, to describe the topics of interest of a journal or the fields of expertise of a scholar, MAG only infers the relations between a publication and its concepts directly and leaves all others to be indirectly aggregated through publications.
- MAG has created its own concept taxonomy solely from the document collection.
- Concept detection is a natural language understanding problem.
- In MAG the package, called Language Similarity, provides a function with which the semantic similarity of two text paragraphs can be quantified using the embedding models trained from the publications in the corresponding MAG version. This function in turn serves as a mixture component for another function that, for any paragraph, returns a collection of top concepts detected in the paragraph that exceed a given threshold. Details: ( https://www.aclweb.org/anthology/P18-4015.pdf )

Example: https://academic.microsoft.com/home
