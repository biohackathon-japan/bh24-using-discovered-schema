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
    orcid: 0000-0000-0000-0000
  - name: Jerven Bolleman
    affiliation: 2
    orcid: 0000-0000-0000-0000
  - name: Jose Emilio Labra Gayo
    affiliation: 1
    orcid: 0000-0000-0000-0000
  - name: Yasunori Yamamoto
    affiliation: 2
    orcid: 0000-0002-6943-6887
  - name: Andra Waagmaaster
    affiliation: 2
    orcid: 0000-0000-0000-0000
  - name: Kozo Nishida
    affiliation: 3
    orcid: 0000-0001-8501-7319
  - name: Hikaru Nagazumi
    affiliation: 2
    orcid: 0000-0000-0000-0000
  - name: Núria Queralt Rosinach
    affiliation: 2
    orcid: 0000-0000-0000-0000
  - name: Gos Micklem
    affiliation: 2
    affiliation: 4
    orcid: 0000-0002-6883-6168
  - name: Chunlei Wu
    affiliation: 2
    orcid: 0000-0000-0000-0000
affiliations:
  - name: Weso Research Group, University of Oviedo, Spain
    index: 1
  - name: Database Center for Life Science (DBCLS), University of Tokyo Kashiwa-no-ha Campus Station Satellite 6F. 178-4-4 Wakashiba, Kashiwa-shi, Chiba, Japan
    index: 2
  - name: Department of Biotechnology and Life Science, Tokyo University of Agriculture and Technology, 2-24-16 Nakacho, Koganei-shi, Tokyo, 184-8588, Japan
    index: 3
  - name: Department of Genetics, University of Cambridge, Cambridge CB2 3EH, United Kingdom
    index: 4
  
    
date: 04 September 2024
cito-bibliography: paper.bib
event: BH24
biohackathon_name: "DBCLS BioHackathon 2024"
biohackathon_url:   "http://2024.biohackathon.org/"
biohackathon_location: "Fukushima, Japan, 2024"
group: Project 26
# URL to project git repo --- should contain the actual paper.md:
git_url: https://github.com/biohackrxiv/publication-template
# This is the short authors description that is used at the
# bottom of the generated paper (typically the first two authors):
authors_short: First Author \emph{et al.}
---


# Introduction

RDF shape languages, such as SHACL and ShEx, are based on the concept of "shape." A shape describes the expected topology around specific types of nodes within an RDF graph. These shapes can be combined into schemas that describe the various local topologies expected around different node types, allowing for the expression of complex patterns that fully describe the topology of a given RDF source. Such schemas have primarily been used for two main purposes: validation and description of RDF data.

On the one hand, a source and a schema can be processed by validation software to check whether the graph structure conforms to the patterns defined by the shapes, producing validation reports if discrepancies are found. These reports can be used by data maintainers to take corrective actions to preserve the graph's structure. On the other hand, schemas can help users understand the expected structure of a dataset, enabling them to add or edit information while maintaining the desired structure, or enabling them to write appropriate queries.

Writing and maintaining RDF schemas can be a time-consuming task, especially when many different shapes are involved. This often requires the expertise of domain specialists. To address this challenge, several approaches for automatic schema discovery have been proposed (CITE sheXer, Jerven’s [tool name! how to cite? @Jerven], Consolidation, Astrea, QSE...). These approaches analyze existing RDF data to extract or infer expected structures and express them as RDF shapes. Although automatically generated shapes may not be as accurate as those created by humans — particularly in complex data models requiring advanced features such as negation or disjunction — they can be quite effective in simpler models or as a draft that domain experts can later refine.


A schema is essentially a machine-readable way to express an RDF data model. This model can capture information such as the properties that should connect different node types, the cardinality of those relationships, or the expected datatypes for literals in various contexts. Schemas can also be annotated with ad-hoc RDF properties to express additional concepts at different levels (e.g. at the shape level or within specific constraints).

Although the traditional uses of RDF schemas are validation and description, their expressiveness and format make them suitable for a variety of other purposes. In this report, we describe several use cases for RDF shapes, particularly focusing on automatically extracted schemas. These use cases were discussed and/or partially implemented by the authors during the 2024 edition of the DBCLS BioHackathon in Fukushima.



# Use cases

This section describes and discusses the use cases identified for automatically discovered schemas. We categorize these use cases into four main areas: data modeling for complex architectures, SPARQL, mappings to other syntaxes, and visualizations.

## Data model for complex architectures

Many applications built using Domain-Driven-Design (DDD) architectures, especially those centered around data models, such as hexagonal architecture, rely not only on the data itself but also on data models or schemas as a starting point for building other components or services in the system. We have identified that shape schemas could be an effective mechanism for describing the expected data structures in such models. When the data is RDF (or can be transformed into RDF), shape schemas could replace other ad-hoc alternatives in these applications, providing the necessary expressiveness and facilitating the adoption of semantic web best practices.

During this BioHackathon, we discussed the adoption of ShEx schemas as a data model description in two Life Sciences platforms:

*Intermine*: InterMine (http://intermine.org) is a framework to build large-scale integrated data resources (e.g. https://www.humanmine.org, https://www.flymine.org). InterMine is a model-driven system in the sense that the front and back-ends configure themselves based on a (currently XML) description of the data model: there is extensive use of automatic code generation. InterMine was designed to enable biological data to be queried flexibly without learning a database query language but also, through RESTful APIs, to enable the use of the data by other programs. Code is generated automatically for any query, to be used with Python, Ruby, Perl, Java and Javascript client libraries.

Central to InterMine's interface is the ability to use lists of biological entities within queries and to upload, combine, save and export these lists in flexible ways. As lists can be created from query outputs and used within queries, iterative analysise is facilitated. 

Another feature of InterMine's interface are "template queries".  The templates are a library of simple fill-out-the-form interfaces. They can be used to solve routine queries, or as starting points to build more complex queries within the query builder. A template can be built from a query using the query builder, by providing a title, description and default values. Templates automatically provide corresponding web services. ShEx and its facility to include machine-readable comments provides the potential to provide a static description of any template query and this potential was investigated in another BioHackathon 2024 project (@Jose ***).

Currently InterMine has a hand-written core data model, which includes the Sequence Ontology (***), and when different data sources are loaded they can, if needed, bring further additions to this model. During BioHackathon 2024 we explored the potential for sheXer to infer a data model that could be used to configure an InterMine database. This is of interest as it could enable the InterMine user interface (BlueGenes ***) to work on any SPARQL endpoint. Work at earlier BioHackathons provided useful starting material: proof-of-concept work at the 2023 BioHackathon (****) and subsequently by Francois Belleau (BOSC REF***) has established a version of the FlyMine database in the QLever (****) backend, kindly hosted by Hannah Bast (****). Further work (*** domestic hackathon) generated proof-of-concept code that translates the FlyMine template query library into SPARQL that runs on this endpoint. 

Here we were able to use sheXer, an automatic shape extractor, to mine the FlyMine-QLever endpoint, and to assess that the model inferred could be used to construct a suitable model for use within InterMine, though code to do this has not yet been written. Thus in future we can in principle 1) run sheXer on a SPARQL endpoint to infer the schema; 2) generate an InterMine model file from the inferred schema; 3) re-write queries generated by the InterMine user interface into SPARQL that can 4) be run on the original SPARQL endpoint and 5) write code that translates the results into the JSON that the user interface is expecting. This would generate a complete proof-of-concept of using the BlueGenes InterMine user interface on top of a SPARQL endpoint. We plan to use the above FlyMine-QLever endpoint as a test case because the underlying data model is understood and we can compare performance to that of the existing FlyMine database. It is already clear from running the most complex FlyMine template query on the FlyMine-QLever endpoint that performance of QLever is excellent.

Thus we have demonstrated that it is possible to infer an equivalent description of the InterMine data model and have a plan to develop the use of the InterMine BlueGenes user interface on arbitrary SPARQL endpoints.

*BioThings*: Biothings is [...] @Chunlei.

The current data models used in BioThings are expressed in a JSON representation. While this approach is sufficient for the necessary services, it is not RDF. However, we found that transforming this representation into ShEx schemas is feasible. This transformation offers two advantages: the data could be made fully compatible with semantic web technologies, and the ShEx schemas could be used with schema validators for data maintenance.

## SPARQL

We have identified that shape schemas can be utilized in several tasks related to RDF data exposed via a SPARQL endpoint:

* SPARQL generation. A SPARQL query is essentially a graph pattern with gaps. Since shapes can be also considered as graph pattern definitions, transforming shapes into SPARQL queries is mainly a matter of mapping shape syntax elements to SPARQL elements. A preliminary version of this mapping has been implemented in the Rudof library and tested in various scenarios during this BioHackathon. Generating complex SPARQL queries involving information from different shapes or even different sources is feasible as long as the connections between shapes are defined in the schema's constraints. @Nuria

Such queries have also been used as input for another project initiated during this BioHackathon, which aims to generate SPARQL queries from natural language inputs using Large Language Models (LLMs). @Jose

Several BioHackathon participants expressed interest in this approach to expedite processes involving frequent SPARQL query writing. @Nuria

* SPARQL validation. Just as shape elements can be mapped to SPARQL query elements, certain elements of SPARQL queries can be mapped to shape schemas. By doing so, SPARQL queries can be semantically validated to some extent before execution, avoiding the execution of queries that, while syntactically correct, would produce no useful results due to mismatched graph patterns. For example, if a variable in a SPARQL query is declared to have a certain type $t$, and it is connected to other query elements using a property $p$, but a shape describing nodes of type $t$ indicates that such nodes are not expected to be connected using $p$, a warning could be generated before executing the query, as it is known in advance that the result set will be empty. @Hikaru

During this BioHackathon, a prototype for validating queries using ShEx schemas was implemented [HIKARU'S SOFTWARE HERE @Hikaru].

* SPARQL term suggestion. The reasoning used for SPARQL validation can also be applied to generate suggestions while writing SPARQL queries. If it is possible to infer that a certain element in a query should conform to a shape’s topology, suggestions can be made to help the user write adequate properties or datatypes.

## Mappings to other syntaxes

While we dedicated a section to the generation and validation of SPARQL queries, the broader idea of mapping schemas to other types of outputs is also significant.

During this BioHackathon, we identified another use case for shape schemas related to RDF-Config (LINK HERE). RDF-Config is [...]. Written in YAML, RDF-Config describes [...]. All the information needed to generate an RDF-Config file can be found in shape schemas generated by extractors like sheXer. Thus, these schemas could support processes involving RDF-Config, including [...]. @Yasunori

Although shape schemes cannot be directly mapped to VOID data, we've identified that the statistical analysis performed by the sheXer extractor could also be leveraged for generating VOID data, in addition to creating shape schemes. @Jerven


## Visualizations

Shape schemas can be useful for describing the structure of data sources for users familiar with semantic web technologies or those with experience in formal languages and data modeling.

There are existing approaches that transform shape schemes into UML diagrams. These representations, which use simple boxes connected by lines, provide a quick overview of the data structure in a graph, benefiting both technically skilled users and those less familiar with the concepts. However, these visualizations are most effective when dealing with relatively simple schemas with fewer shapes. When the number of shapes increases, the resulting images can become too complex for users to interpret or display effectively. @Andra

During this BioHackathon, we proposed alternative visualizations to address this issue. All the proposed methods focus on viewing a subset of the schema while allowing users to explore different regions of the overall structure. The three main proposals are as follows:

- Graph-like visualization for Python Jupyter using yFiles. This library offers various graph-based representations of schemas, which can be more or less interactive. While primarily used within Python Jupyter notebooks, static views of the graphs can be exported to different formats for reuse in other contexts. @Kozo

- Partial html views. This visualization style involves creating HTML views centered on a seed shape, displaying only the shapes in its immediate neighborhood. Each shape connected to the seed would be clickable, allowing the user to navigate to another HTML view with a different seed shape and thus exploring the whole schema. @Jose

- Interactive web views with zoom in/out capabilities. This proposal extends the previous concept. The core idea remains the same: enabling exploration of large schemas by focusing on local sections. However, in this case, the sections are not limited to static representations of a shape and its immediate neighborhood. Instead, dynamically generated subgraphs of related shapes, including those not directly connected to the seed, are displayed. The user would be able to configure both the zoom level and the area to focus on, providing classic map visualizations. @Yasunori


## Conclusions 

During this BioHackathon, we identified, discussed, and partially implemented several potential use cases for shape schemas. A common theme among these efforts is the reuse of shape schemas to automate the creation of additional products or to support processes dependent on the data model.

By using shape extractors such as sheXer or [Jerven's] to generate these shape schemas, we could fully automate these processes, eliminating the need to manually write or maintain schemas, as they can be directly derived from the data itself.

## Acknowledgements

Many thanks are due to the organisers of BioHackathon 2024 and to the DBCLS for organising an extremely useful event to test out new ideas, build new collaborations and reinforce existing ones.[...]

## References

[...]
