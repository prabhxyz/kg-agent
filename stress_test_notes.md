# 📝 Stress Test TODOs

---

###Stress Test #1:
Dataset: GeoQuestions1089
Size: 1,089 triples (small-scale)

Plan:
The goal was to see how well the knowledge graph agent could handle a small, clean dataset with a simple structure. GeoQuestions1089 has clear columns: Question, Query, and Answer, so it was a good candidate to test basic schema inference and triple extraction.


Results:
The model successfully identified logical relationships and generated a clean schema:
[("Question", "is answered by", "Answer"), ("Query", "executes on", "Database"), ("Answer", "is the capital of", "France")]

It created 1,085 instance-level triples from the CSV file.

Schema and data graphs were both visualized without errors, and the sample subgraph made sense structurally.

Observations:
The agent handled the dataset smoothly with no crashes or major issues.

The inferred schema was both relevant and useful for mapping question–answer logic.
Overall Solid Pass


###Stress Test #2:
Dataset: scikit-learn/adult-census-income
Size: ~32,000 triple sets (medium-scale)

Plan:
This test focused on evaluating the agent’s ability to infer meaningful relational structure from a tabular dataset that lacks explicit triples. The goal was to generate a schema using column names, use a language model to predict [head, relation, tail] triples, and then populate a knowledge graph from actual instance-level data. We aimed to verify if the agent could handle semi-structured data and produce a usable, interpretable graph.


Results:
Loaded the dataset successfully from /content/adult.csv.

The LLM initially returned plain-text or malformed JSON, requiring fallback handling.

After refining the parser to accept head, relation, tail formatted lines, 30+ high-quality triples were extracted.

The agent was able to build a directed graph with distinct attribute–value pairs.

A subset of 50 triples was visualized effectively using node type coloring and spring layout.



Observations:
LLM outputs on schema inference can vary significantly in formatting. Plain-text or comma-delimited triples were common.

Initial regex-based parsing failed due to rigid assumptions. A generic CSV-style parser worked more reliably.

The resulting graph was sparse, with many disconnected attribute–value pairs, making full-graph visualization less useful.


Overall, this test confirmed the agent can interpret and visualize mid-scale tabular data, provided robust preprocessing is in place.


###Stress Test #3:
Dataset: KGraph/FB15k-237

Size: ~272K rows

Plan:
This test was designed to evaluate how well the agent handles a large-scale, structured knowledge graph dataset. Unlike GeoQuestions, this one doesn’t need schema inference—it’s already in a head–relation–tail format. The goal was to assess parsing efficiency, graph construction, and whether we could extract meaningful insights like connectivity, subgraph structure, and high-degree bridge nodes.


Results:
Successfully parsed 272,115 triples from train.txt.

Constructed a directed multi-graph with 8 connected components.

Identified key bridge-like nodes such as Deep Learning (degree 56), Machine Learning (degree 52), and Introduction to Machine Learning (degree 31).

Sample subgraphs were visualized without issues.

Graph operations (e.g., connected components, BFS, degree sorting) completed efficiently even at this scale.



Observations:
The pipeline handled the large dataset well—no memory issues or major slowdowns when processing subsets.

High-degree nodes like academic concepts and years (e.g., 2018, Neural Networks) stood out as potential anchors or hubs in the graph.

The separation into 8 components suggests either isolated domains or underlinked data sections, which might be useful for downstream tasks like link prediction or entity clustering.

Overall, this confirmed the pipeline is scalable and stable on large real-world Datasets.

###Stress Test #4:
Dataset: VLyb/YAGO3-10

Size: 1 Million Rows

Plan:
This stress test was designed to assess the agent’s ability to handle an extremely large-scale, highly relational knowledge graph formatted as head–relation–tail triples. The goal was to:

Efficiently parse and ingest over 1 million rows from train.csv

Construct a knowledge graph from the parsed data

Visualize a portion of the graph to assess structure, entity reuse, and relation density

Confirm that graph traversal and sampling logic (e.g., BFS-based subgraphing) works as intended at scale


Results:
Successfully loaded 1,089,040 triples from train.csv

Parsed all rows into (head, relation, tail) format with no errors

Full graph visualization using a 1000-edge sample rendered successfully, though it was very dense and text-heavy


Observations:
The dataset naturally exhibits strong graph structure, with many reused entities and overlapping relations

Full-graph visualization is impractical at this scale due to label density — even a 1000-edge sample resulted in a highly cluttered plot

The subgraph sampling technique proved valuable for meaningful inspection and debugging, as it allows zoomed-in views of local structure




---
