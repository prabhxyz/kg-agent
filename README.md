# 📝 Stress Test TODOs
---

## Stress Test #1 – **GeoQuestions1089** (1,089 triples)

### Plan
- Test basic schema inference and triple extraction on a small, clean dataset with straightforward columns (`Question`, `Query`, `Answer`).

### Results
- The model identified logical relationships and generated the following schema:
  ```text
  [("Question", "is answered by", "Answer"),
   ("Query", "executes on", "Database"),
   ("Answer", "is the capital of", "France")]
````

* 1,085 instance-level triples were created.
* Schema and data graphs visualized correctly; sample subgraph was structurally sound.

### Observations

* No crashes or major issues.
* Inferred schema was relevant and useful.

**Overall:** **Solid Pass**

---

## Stress Test #2 – **scikit-learn/adult-census-income** (\~32 000 triple sets)

### Plan

* Infer a schema from column names, use an LLM to predict `[head, relation, tail]` triples, and populate a knowledge graph from the tabular data.

### Results

* Dataset loaded from `/content/adult.csv`.
* LLM initially returned plain-text/malformed JSON; fallback handling added.
* After parser refinement, 30 + high-quality triples extracted.
* Directed graph built; 50-triple subset visualized with node-type coloring and spring layout.

### Observations

* LLM output formats varied (plain text, comma-delimited).
* Rigid regex parsing failed; generic CSV-style parsing more reliable.
* Resulting graph was sparse with many disconnected attribute–value pairs; full-graph visualization less useful.

**Overall:** Agent can interpret and visualize mid-scale tabular data when robust preprocessing is applied.

---

## Stress Test #3 – **KGraph/FB15k-237** (\~272 000 rows)

### Plan

* Evaluate parsing efficiency, graph construction, and insight extraction on a large head–relation–tail dataset (no schema inference required).

### Results

* Parsed 272 115 triples from `train.txt`.
* Directed multi-graph with 8 connected components constructed.
* High-degree nodes identified (e.g., *Deep Learning* 56, *Machine Learning* 52, *Introduction to Machine Learning* 31).
* Sample subgraphs visualized; graph algorithms (connected components, BFS, degree sorting) ran efficiently.

### Observations

* No memory issues or slowdowns.
* High-degree academic concepts and years acted as hubs.
* Eight components hint at isolated domains or under-linked sections—useful for tasks like link prediction or clustering.

**Overall:** Pipeline is scalable and stable on large real-world datasets.

---

## Stress Test #4 – **VLyb/YAGO3-10** (≈1 000 000 rows)

### Plan

1. Parse and ingest > 1 M head–relation–tail rows from `train.csv`.
2. Construct a knowledge graph.
3. Visualize a portion to assess structure, entity reuse, and relation density.
4. Confirm traversal and sampling logic at scale.

### Results

* Loaded **1 089 040** triples with zero parsing errors.
* Full-graph visualization (1 000-edge sample) rendered successfully—very dense/text-heavy.

### Observations

* Strong underlying graph structure with extensive entity/relation reuse.
* Full-graph views are impractical at this scale; even 1 000 edges produced clutter.
* Subgraph sampling invaluable for inspection and debugging.

**Overall:** Pipeline handles extreme-scale data; subgraph sampling is essential for meaningful visualization.