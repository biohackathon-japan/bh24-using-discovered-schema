---
title: 'BioHack24 report: Using discovered RDF schemes: a compilation of potential use cases for shapes reusage'
title_short: 'BioHack24 report: Using discovered RDF schemes'
tags:
  - RDF
  - ShEx
  - Schema
  - Automatic extraction
  - Templates
  - Data modelling
authors:
  - name: Daniel Fernández-Álvarez
    affiliation: 1
    orcid: 0000-0002-8666-7660
  - name: Jerven Bolleman
    affiliation: 2
    orcid: 0000-0002-7449-1266
  - name: Jose Emilio Labra Gayo
    affiliation: 1
    orcid: 0000-0001-8907-5348
  - name: Yasunori Yamamoto
    affiliation: 3
    orcid: 0000-0002-6943-6887
  - name: Andra Waagmaaster
    affiliation: 4
    orcid: 0000-0001-9773-4008
  - name: Kozo Nishida
    affiliation: 5
    orcid: 0000-0001-8501-7319
  - name: Hikaru Nagazumi
    affiliation: 6
  - name: Núria Queralt-Rosinach
    affiliation: 7
    orcid: 0000-0003-0169-8159
  - name: Gos Micklem
    affiliation: 3,8
    orcid: 0000-0002-6883-6168
  - name: Chunlei Wu
    affiliation: 9
    orcid: 0000-0002-2629-6124
affiliations:
  - name: Weso Research Group, University of Oviedo, Spain.
    index: 1
  - name: Swiss-Prot group, SIB Swiss Institute of Bioinformatics, Geneva, Switzerland.
    index: 2
  - name: Database Center for Life Science (DBCLS), University of Tokyo Kashiwa-no-ha Campus Station Satellite 6F. 178-4-4 Wakashiba, Kashiwa-shi, Chiba, Japan.
    index: 3
  - name: Department of Medical Informatics, University of Amsterdam, The Netherlands.
    index: 4
  - name: Department of Biotechnology and Life Science, Tokyo University of Agriculture and Technology, 2-24-16 Nakacho, Koganei-shi, Tokyo, 184-8588, Japan.
    index: 5
  - name: Waseda University, Tokyo, Japan.
    index: 6
  - name: Department of Human Genetics, Leiden University Medical Center, 2333 ZA Leiden, The Netherlands.
    index: 7
  - name: Department of Genetics, University of Cambridge, Cambridge CB2 3EH, United Kingdom.
    index: 8
  - name: Department of Integrative, Structural and Computational Biology, Scripps Research, La Jolla, California.
    index: 9

    
date: 04 September 2024
cito-bibliography: paper.bib
event: BH24
biohackathon_name: "DBCLS BioHackathon 2024"
biohackathon_url:   "http://2024.biohackathon.org/"
biohackathon_location: "Fukushima, Japan, 2024"
group: Using (discovered) schemes
# URL to project git repo --- should contain the actual paper.md:
git_url: https://github.com/biohackathon-japan/bh24-using-discovered-schema/blob/main/paper/paper.md
# This is the short authors description that is used at the
# bottom of the generated paper (typically the first two authors):
authors_short: Daniel Fernández-Álvarez, Jerven Bolleman \emph{et al.}
---


# Introduction

RDF shape languages, such as SHACL [@citesAsAuthority:SHACLSpec] and ShEx [@citesAsAuthority:prud2014shape], are based on the concept of "shape." A shape describes the expected topology around specific types of nodes within an RDF graph. These shapes can be combined into schemas that describe the various local topologies expected around different node types, allowing for the expression of complex patterns that fully describe the topology of a given RDF source. Such schemas have primarily been used for two main purposes: validation and description of RDF data.

On the one hand, a source and a schema can be processed by validation software to check whether the graph structure conforms to the patterns defined by the shapes, producing validation reports if discrepancies are found. These reports can be used by data maintainers to take corrective actions to preserve the graph's structure. On the other hand, schemas can help users understand the expected structure of a dataset, enabling them to add or edit information while maintaining the desired structure, or enabling them to write appropriate queries.

Writing and maintaining RDF schemas can be a time-consuming task, especially when many different shapes are involved. This often requires the expertise of domain specialists. To address this challenge, several approaches for automatic schema discovery have been proposed [@citesAsAuthority:fernandez2022shexer; @citesAsAuthority:Bolleman2023, @citesAsAuthority:fernandez2024extracting; @citesAsAuthority:cimmino2020astrea; @citesAsAuthority:rabbani2023extraction; @citesAsAuthority:keely2023shaclgen; @citesAsAuthority:boneva2019shape; @citesAsAuthority:mihindukulasooriya2018rdf; @citesAsAuthority:groz2022inference; @citesAsAuthority:omran2020towards; @citesAsAuthority:spahiu2018towards; @citesAsAuthority:rabbani2023shactor]. These approaches analyze existing RDF data to extract or infer expected structures and express them as RDF shapes. Although automatically generated shapes may not be as accurate as those created by humans — particularly in complex data models requiring advanced features such as negation or disjunction — they can be quite effective in simpler models or as a draft that domain experts can later refine.

A schema is essentially a machine-readable way to express an RDF data model. This model can capture information such as the properties that should connect different node types, the cardinality of those relationships, or the expected datatypes for literals in various contexts. Schemas can also be annotated with ad-hoc RDF properties to express additional concepts at different levels (e.g., at the shape level or within specific constraints).

Although the traditional uses of RDF schemas are validation and description, their expressiveness and format make them suitable for a variety of other purposes and challenges in data interoperability and exploration. In this report, we describe several use cases for RDF shapes, particularly focusing on automatically extracted schemas. These use cases were discussed and/or partially implemented by the authors during the 2024 edition of the [DBCLS BioHackathon](https://2024.biohackathon.org/) in Fukushima.

# Use cases

This section describes and discusses the use cases identified for automatically discovered schemas. We categorize these use cases into four main areas: data modeling for complex architectures, SPARQL, mappings to other syntaxes, and visualizations.

## Data modeling for complex architectures

Many applications built using Domain-Driven-Design (DDD) architectures, especially those centered around data models, such as hexagonal architecture [@citesAsAuthority:cockburn2005hexagonal], rely not only on the data itself but also on data models or schemas as a starting point for building other components or services in the system. We have identified that shape schemas could be an effective mechanism for describing the expected data structures in such models. When the data is RDF (or can be transformed into RDF), shape schemas could replace other ad-hoc alternatives in these applications, providing the necessary expressiveness and facilitating the adoption of semantic web best practices.

During this BioHackathon, we discussed the adoption of ShEx schemas as a data model description in two Life Sciences platforms:

*Intermine*: [InterMine](http://intermine.org) is a framework to build large-scale integrated data resources (e.g. [HumanMine](https://www.humanmine.org), [FlyMine](https://www.flymine.org)). InterMine is a model-driven system in the sense that the front and back-ends configure themselves based on a (currently XML) description of the data model: there is extensive use of automatic code generation. InterMine was designed to enable biological data to be queried flexibly without learning a database query language but also, through RESTful APIs, to enable the use of the data by other programs. Code is generated automatically for any query, to be used with Python, Ruby, Perl, Java and Javascript client libraries.

Central to InterMine's interface is the ability to use lists of biological entities within queries and to upload, combine, save and export these lists in flexible ways. As lists can be created from query outputs and used within queries, iterative analysise is facilitated. 


Another feature of InterMine's interface are "template queries".  The templates are a library of simple fill-out-the-form interfaces. They can be used to solve routine queries, or as starting points to build more complex queries within the query builder. A template can be built in minutes from a query using the query builder, by providing a title, description and default values. Templates automatically provide corresponding web services. ShEx and its facility to include machine-readable comments provides the potential to provide a static description of any template query and this potential was investigated in another [BioHackathon 2024 project](https://github.com/biohackathon-japan/bh24-sparql-schema-conversions/blob/main/paper/paper.md).


Currently InterMine has a hand-written core data model, which includes the [Sequence Ontology](http://www.sequenceontology.org) and when different data sources are loaded they can, if needed, bring further additions to this model. During BioHackathon 2024 we explored the potential for [sheXer](https://github.com/DaniFdezAlvarez/shexer), an automatic shape extractor, to infer a data model that could be used to configure an InterMine database. This is of interest as it could enable the new InterMine user interface, [BlueGenes](http://intermine.org/im-docs/docs/webapp/bluegenes/index) to work on any SPARQL endpoint. Work at earlier BioHackathons provided useful starting material: proof-of-concept work on [SPARQL services for InterMine databases](https://osf.io/preprints/biohackrxiv/dpnry) at the [DBCLS BioHackathon 2023](https://2023.biohackathon.org/) and subsequently by Francois Belleau [A QLever SPARQL endpoint for InterMine databases](https://www.youtube.com/watch?v=YuCdruCgX_Y) for [BOSC2024](https://www.open-bio.org/events/bosc-2024/) has established a version of the FlyMine database in the [QLever](https://github.com/ad-freiburg/qlever) backend [@citesAsAuthority:bast2017qlever], with his [FlyMine-QLever endpoint](https://qlever.cs.uni-freiburg.de/bio2rdf) now kindly hosted by QLever's lead, Hannah Bast. Further work (at the 2023 DBCLS Domestic BioHackathon) generated proof-of-concept code that translates the FlyMine template query library into SPARQL that runs on this endpoint. 

Here we were able to use sheXer, to mine the FlyMine-QLever endpoint, and to assess that the model inferred could be used to construct a suitable model for use within InterMine, though code to do this has not yet been written. Thus in future we can in principle 1) run sheXer on a SPARQL endpoint to infer the schema; 2) generate an InterMine model file from the inferred schema; 3) re-write queries generated by the InterMine user interface into SPARQL that can 4) be run on the original SPARQL endpoint and 5) write code that translates the results into the JSON that the user interface is expecting. This would generate a complete proof-of-concept of using the BlueGenes InterMine user interface on top of a SPARQL endpoint. We plan to use the above FlyMine-QLever endpoint as a test case because the underlying data model is understood and we can compare performance to that of the existing FlyMine database. It is already clear from running the most complex FlyMine template query on the FlyMine-QLever endpoint that performance of QLever is excellent.

Thus we have demonstrated that it is possible to infer an equivalent description of the InterMine data model and have a plan to develop the use of the InterMine BlueGenes user interface on arbitrary SPARQL endpoints.

*BioThings*: [BioThings](https://biothings.io/) is an API ecosystem that provides information on a wide range of biomedical entities represented in the biological knowledge space, such as genes, genetic variants, drugs, chemicals, diseases, and more.

The current internal data models in BioThings are expressed in JSON. We found that converting these models into ShEx schemas is entirely feasible. Moreover, we determined that shape schemas are expressive enough to supply information to BioThings services that currently rely on the JSON models. While the existing JSON-based approach is sufficient to support current services, using shape schemas to define data models offers two key advantages: first, the data can be made fully compatible with Semantic Web technologies; second, shape schemas can be leveraged with existing schema validators for improved data maintenance.

## SPARQL

We have identified that shape schemas can be utilized in several tasks related to RDF data exposed via a SPARQL endpoint:

* SPARQL generation. The generation of SPARQL queries is a standard step for data retrieval and exploration of RDF data and knowledgebases. However, this is a time consuming task for users since it requires to understand the schema of RDF knowledge graphs. During the BioHackathon, we hypothesized that automated discovery of RDF schemas would facilitate this task. A SPARQL query is essentially a graph pattern with gaps. Since shapes can be also considered as graph pattern definitions, transforming shapes into SPARQL queries is mainly a matter of mapping shape syntax elements to SPARQL elements. A preliminary version of this mapping has been implemented in the [Rudof library](https://github.com/rudof-project/rudof) and tested in various scenarios during this BioHackathon. Generating complex SPARQL queries involving information from different shapes or even different sources is feasible as long as the connections between shapes are defined in the schema's constrains. The generation of complex queries is a common scenario for bioinformaticians and data scientists, ranging from the annotation of owned RDF data with external public sources to generate federated SPARQL queries, i.e., traversing data across different RDF schemas exposed from different SPARQL endpoints.

Such queries have also been used as input for another project initiated during this BioHackathon, which aims to [generate SPARQL queries from natural language inputs using Large Language Models](https://github.com/biohackathon-japan/bh24-llm-sparql).

Several BioHackathon participants expressed interest in this approach to expedite processes involving frequent SPARQL query writing. In particular, to support the generation of knowledge graphs for specific downstream tasks such as machine learning. This may at least have a two-fold benefit of: (1) facilitating the integrated use of data; and (2) the integration of Semantic Web and data mining models. We explored the challenging example of federated querying from three different sources to retrieve metabolite centric drug information. From the query prepared during the DBCLS BioHackathon 2023 (see the query [here](https://nuriaqueralt.github.io/bh23-onlinebook/metabolite_subsetting.html)), which traverses UniProt, Rhea and IDSM sources, we got their three RDF discovered schema shapes. Although this was a promising step towards automated generation of federated SPARQL queries, the specific connection needed between shapes depends on the use case, i.e., on the type of data we want to retrieve. Potentially, these schema constrains could be pointed by the user through the necessary semantics of the SPARQL elements to be retrieved. But this is an open question that requires further investigation and discussion.

* SPARQL validation. Just as shape elements can be mapped to SPARQL query elements, certain elements of SPARQL queries can be mapped to shape schemas. By doing so, SPARQL queries can be semantically validated to some extent before execution, avoiding the execution of queries that, while syntactically correct, would produce no useful results due to mismatched graph patterns. For example, if a variable in a SPARQL query is declared to have a certain type $t$, and it is connected to other query elements using a property $p$, but a shape describing nodes of type $t$ indicates that such nodes are not expected to be connected using $p$, a warning could be generated before executing the query, as it is known in advance that the result set will be empty.

During this BioHackathon, [a prototype for validating queries using ShEx schemas](https://github.com/scott2121/sparql_type_checker) was implemented.

* SPARQL term suggestion. The reasoning used for SPARQL validation can also be applied to generate suggestions while writing SPARQL queries. If it is possible to infer that a certain element in a query should conform to a shape’s topology, suggestions can be made to help the user write adequate properties or datatypes.

## Mappings to other syntaxes

While we dedicated a section to the generation and validation of SPARQL queries, the broader idea of mapping schemas to other types of outputs is also significant.

During this BioHackathon, we identified an additional use case for shape schemas in relation to [RDF-Config](https://github.com/dbcls/rdf-config/). RDF-Config is a tool to generate SPARQL queries, schema diagrams, and files required for projects such as [Grasp](https://github.com/dbcls/grasp) or [TogoStanza](http://togostanza.org/), ammong others. RDF-Config relies on YAML files that describe various structural aspects of a given RDF source. Much of the information needed to create an RDF-Config file can be derived from shape schemas generated by extractors like sheXer. Consequently, these extracted schemas could be mapped directly to RDF-Config files. This would enable the generation of all RDF-Config-related products without the need to manually write YAML input files or possess prior knowledge of the target source's structure.

Although shape schemes cannot be directly mapped to [VoID](https://www.w3.org/TR/void/) data, we have identified that the statistical analysis performed by sheXer's extraction process could also be leveraged for generating VOID data, in addition to creating shape schemes. 

The direction from VoID to shapes such as ShEX is also possible and can be performed using [void2shapes](https://github.com/shex-consolidator/void2shapes).


## Visualizations

Shape schemas are useful for describing the structure of data sources, particularly for users well-versed in Semantic Web technologies or experienced in formal languages and data modeling. In contrast, users without this background may find it challenging to interpret shape schemas. However, these schemas can serve as input for tools that generate more visually appealing and accessible representations of the underlying information.

For instance, some approaches transform shape schemas into UML diagrams, as seen in tools like [RDFShape](https://rdfshape.weso.es/shexInfo). These representations, featuring simple boxes connected by lines, offer a quick overview of the data structure in a graph, making them useful for both technically skilled users and those with less familiarity with the underlying concepts. However, such visualizations are most effective when applied to relatively simple schemas with a limited number of shapes. As the number of shapes increases, the diagrams can become overly complex, making them difficult for users to interpret or display effectively.

During this BioHackathon, we proposed alternative visualizations to address this issue. All of the proposed methods focus on displaying a subset of the schema, while allowing users to explore different regions of the overall structure. The three main proposals are as follows:

- Graph-like visualization for [Python Jupyter](https://jupyter.org/) using [yFiles](https://www.yworks.com/products/yfiles-graphs-for-jupyter). This library can support various graph-based representations of schemas, which can be more or less interactive. While primarily used within Python Jupyter notebooks, static views of the graphs can be exported to different formats for reuse in other contexts.

- Partial html views. This visualization style involves creating HTML views centered on a seed shape, displaying only the shapes in its immediate neighborhood. Each shape connected to the seed would be clickable, allowing the user to navigate to another HTML view with a different seed shape and thus exploring the whole schema.

- Interactive web views with zoom in/out capabilities. This proposal builds on the previous idea by enhancing interactivity. While it still enables exploration of large schemas by focusing on local sections, the displayed areas are not limited to static representations of a shape and its direct neighbors. Instead, dynamically generated subgraphs of related shapes—including those not directly connected to the seed—are shown. Users can adjust the zoom level and choose which area to explore, similar to navigating a map.


## Conclusions 

During this BioHackathon, we identified, discussed, and partially implemented several potential use cases for shape schemas beyond just validation and description of RDF content. A recurring theme across these efforts was the reuse of shape schemas to automate the generation of additional outputs or to support processes that rely on the underlying data model.

By utilizing shape extractors like sheXer or void2shapes to generate these schemas, we can fully automate various workflows, removing the need for manual schema creation or maintenance, as they can be directly derived from the data itself.

## Acknowledgements

Many thanks are due to the organisers of BioHackathon 2024 and to the DBCLS for organising an extremely useful event to test out new ideas, build new collaborations, and reinforce existing ones.

## References

