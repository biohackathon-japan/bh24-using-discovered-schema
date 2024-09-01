Reusing discovered schema
--------------------------

During this biohackathon, we have been able to find several use cases for automatically discovered schemes. In most cases, there has not been enough time in order to produce a working prototype to cover the whole use case, but we were able to extract sample schemes and discuss plans to use them.

Among others, we have detected, discussed and/or extracted shapes for the following use cases:

- Intermine: Intermine is [...] (Gos). 

A number of elements in this portal are automatically generated from an internal ad-hoc serialization of the data models. However, those data models are written by humans. By using sheXer to produce ShEx shapes, we'd been able to produce a standard schema describing such data models. Also, we have that the expressiveness of ShEx is good enough in order to fully represent all the information required in this use case. Replacing the current internal data model by auto-generating ShEx schemes, that may or may not be reviewd by humans, could improve maintanance times at the time a standard vocabulary is being used.

- BioThings is [...] (Chunlei).

The current data models internally used in BioThings are expressend in a JSON representation. Such approach is currently enough to provide the neccesary services, but its not RDF. Nevertheless, we've found that transforming such representation into ShEx schemas is feasible. This could have two advantages. On the one side, the data could be transformed to be fully compatible with semantic web technologies. On the other hand, the ShEx schemas coudl be used along with schema validators for data maintenance.

- Generation of federated queries (Nuria). 

A number of uses cases require to gather data to create temporal knowledge graphs. KG subsetting/combination for specific purposes, machine learning... A frequent need to create such graphs is using federated queries. Such queries are frequently hard to write. We have found that ShEx schemas could contain the information require to fully generate or assist the generation of SPARQL queries. In different ways: creation of mappings, suggestion of terms, graph pattern validation...

- Rdf-config is [...] (Yasunori).

Rdf-config is based on YAML files. The information contained in those files could be all found in ShEx schemes. Then, transforming the automatically extracted into RDF-config files could improve most of the workflows in which RDF-config is integrated.

- VOID data can be used to describe the contents offered in an RDF dataset or SPARQL endpoint. This tyoe of information can be useful for query optimization, but also for several other purposes (Maybe to be completed by Jerven). ShEx schemas cannot be tranformed into void, even if complete void data can indeed be used to get ShEx schemas. However, during the processes required to get the schemas with the sheXer tool, it is possible to generate void data.


Visualizations
--------------

The current visualizations of ShEx schemas (mostly based on UML or UML-like representations) fail to be really useful for human readers when the decipted schemas contain too many shapes, as the graphic representatio of that becomes too big. Even if we have not been able to produce prototype code with human-friendlier visualizations of ShEx schemes, several promising ideas have been proposed during the biohackathon.

- Kozo: Kozo has suggested to implement a visualization based on yFiles graphs. Once the ShEx schema has been extracted, it could be parsed into a different library (probably Pyshex) to walk its AST representation as object model objects. The connection berween schema, as well as some other data, could be used then for developing a graph-like representation of shape connections.
  
- Jose: Jose suggests to implment several html representations focussed on parts of the graph described by schema relations. Each html view would be centered in a given shape, and only the shapes with direct links from/to it would be represented. Then, huge schemas could be explored partially and more confortably for human readers.
  
- Yasunori: Yasunori suggest an improvement over Jose's proposal. He envisions a map-like representation of schemas, where you can zoom in/out certain regions. The core idea is similar to Jose's one: being able to explore huge schemas by onl considering local parts. But, in this case, those parts would not be limited by static representations of a shape and its inmediate neighborhood, but dinamically generation so the subgraph of related shapes ploted could have other shapes non-conected directly to the seed one, with a configurable depth for exploring the neigborhood. 

