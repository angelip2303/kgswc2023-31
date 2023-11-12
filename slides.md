---
theme: apple-basic
themeConfig:
  paginationX: r
  paginationY: t
  paginationPagesDisabled: [1,31]
layout: intro-image
image: /img/cover.png
---

<div>
<a href="https://angelip2303.github.io" class="font-300">Ángel Iglesias Préstamo</a>
    ·
<a href="https://labra.weso.es/" class="font-300">Jose Emilio Labra Gayo</a>
</div>

<div>
  	<h1 style="font-size: 2.5em">Creating Knowledge Graph Subsets</h1>
  	<p class="font-300">
        Using Pregel to Create Knowledge
        Graphs Subsets Described
        by Non-recursive Shape Expressions
    </p>
</div>

<Pagination classNames="text-gray-300" />

---
layout: section
---

# 🥁 Motivation


---
layout: center
---

# 💡 We know...

- ✅ Knowledge Graphs are a **powerful tool** to represent knowledge.
- ❌ It is hard ensuring their **quality**.
- ❌ They tend to be **huge**.
- 💡 _Creating Subsets_ could solve both issues.

---

# 🤓 How can we achieve that?

<figure>
    <img
        class="mx-auto"
        src="/public/img/general_subset_generator.svg" 
        alt="Subset generator"
    />
    <figcaption> <span> Figure 2: </span> ShEx-based <it> subset </it> generation process for Alan Turing's example </figcaption>
</figure>

---
layout: section
---

# 🔍 The algorithm in a nutshell

---

# 🧮 PSchema

- **P**regel-based **Schema** validation algorithm for Knowledge Graphs.
- Based on a **multithreaded** Pregel.
- **Shape Expressions**.
- Can validate different types of **Knowledge Graphs**.

## 💭 Alternatives

- [wdsub](https://github.com/weso/wdsub)
- [wdumper](https://github.com/bennofs/wdumper)
- [sparkwdsub](https://github.com/weso/sparkwdsub)

---

# ⌨️ The algorithm in detail

```rust
fn pschema(g: Graph, tree: Tree) -> Graph {
    let initial_messages = None;
    let max_iterations = // see the Convergence Lemma
    Pregel(g, max_iterations, initial_messages, send_messages, aggregate_messages, vertex_program)
}

fn send_messages(g: Graph, level: TreeLevel) -> Messages {
    let messages = Vec::new(); // Growable array type
    for shape in level {
        messages.push(shape.validate(g)); // The validate function depends on the type of the Shape Expression
    }
    messages
}

fn aggregate_messages(messages: Messages) -> Messages {
    messages.filter(|message| message != None)
}

fn vertex_program(g: Graph, messages: Messages) -> Graph {
    g.vertices.concatenate(messages)
}
```

---
layout: diagram
---

<h1> 👨‍💻 Pregel<sup>1</sup> </h1>

- **Graph processing** framework.
- Bulk Synchronous Parallel.
- Uses message passing.
- _Thinking like a vertex_.

<Footnotes separator>
    <Footnote :number=1>
    Pregel as implemented in <a href="https://github.com/weso/pregel-rs"> pregel-rs </a>
    </Footnote>
</Footnotes>

::right::

<figure>
    <img
        class="mx-auto"
        src="/public/img/pregel.svg" 
        alt="Pregel model"
    />
    <figcaption> <span> Figure 3: </span> Pregel model as implemented in pregel-rs </figcaption>
</figure>

---
layout: diagram-header
---

# 🤔 How are Shape Expressions represented?

::left::

- Shapes are represented as a tree.
- Each node represents a **Shape**.
- **Validation** is done by traversing the tree.
- The validating behaviour of Shapes is key.

::right::

<figure>
    <img
        class="mx-auto"
        src="/public/img/tree.svg" 
        alt="Shape Expression tree"
    />
    <figcaption> <span> Figure 4: </span> Shape Expression tree </figcaption>
</figure>

---
layout: diagram-header
---

# 🚶 _Reverse_ Level order traversal

::left::

- **_Reverse_ level order traversal** visits the nodes of a tree level by level, but in _reverse_ order.

- For each _superstep_ of the Pregel, the algorithm traverses **one** level of the tree.

::right::

<img
    class="m-auto"
    src="/public/img/traversal.svg" 
    alt="Tree traversal as implemented in PSchema"
    width="400"
/>

---
layout: full
---

# 0️⃣ The algorithm in action -- Initial Messages

<img
    class="m-auto"
    src="/public/img/initial_step.svg" 
    alt="PSchema algorithm trace"
    width="750"
/>

---
layout: big-diagram
---

# 1️⃣ 1<sup>st</sup> superstep -- Send Messages

::big::

<img
    class="mx-auto"
    src="/public/img/send_messages_first.svg" 
    alt="PSchema algorithm trace"
/>

::small::

<img
    class="mx-auto"
    src="/public/img/first_tree.svg" 
    alt="Shape Expression tree"
/>

::bottom::

<Footnotes separator>
    <Footnote :number=1>
        For simplicity, only the nodes that are valid are highlighted. However, all the nodes will send messages to their neighbors in the <it> destination </it> to <it> source </it> direction.
    </Footnote>
</Footnotes>

---
layout: big-diagram
---

# 1️⃣ 1<sup>st</sup> superstep -- Aggregate Messages

::big::

<img
    class="mx-auto"
    src="/public/img/agg_messages_first.svg" 
    alt="PSchema algorithm trace"
/>

::small::

<img
    class="mx-auto"
    src="/public/img/first_tree.svg" 
    alt="Shape Expression tree"
/>

---
layout: big-diagram
---

# 1️⃣ 1<sup>st</sup> superstep -- Vertex Program

::big::

<img
    class="mx-auto"
    src="/public/img/vertex_program_first.svg" 
    alt="PSchema algorithm trace"
/>

::small::

<img
    class="mx-auto"
    src="/public/img/first_tree.svg" 
    alt="Shape Expression tree"
/>

---
layout: big-diagram
---

# 2️⃣ 2<sup>nd</sup> superstep -- Send Messages

::big::

<img
    class="mx-auto"
    src="/public/img/send_messages_second.svg" 
    alt="PSchema algorithm trace"
/>

::small::

<img
    class="mx-auto"
    src="/public/img/second_tree.svg" 
    alt="Shape Expression tree"
/>

---
layout: big-diagram
---

# 2️⃣ 2<sup>nd</sup> superstep -- Aggregate Messages

::big::

<img
    class="mx-auto"
    src="/public/img/agg_messages_second.svg" 
    alt="PSchema algorithm trace"
/>

::small::

<img
    class="mx-auto"
    src="/public/img/second_tree.svg" 
    alt="Shape Expression tree"
/>


---
layout: big-diagram
---

# 2️⃣ 2<sup>nd</sup> superstep -- Vertex Program

::big::

<img
    class="mx-auto"
    src="/public/img/vertex_program_second.svg" 
    alt="PSchema algorithm trace"
/>

::small::

<img
    class="mx-auto"
    src="/public/img/second_tree.svg" 
    alt="Shape Expression tree"
/>


---
layout: big-diagram
---

# 3️⃣ 3<sup>rd</sup> superstep -- Send Messages

::big::

<img
    class="mx-auto"
    src="/public/img/send_messages_third.svg" 
    alt="PSchema algorithm trace"
/>

::small::

<img
    class="mx-auto"
    src="/public/img/third_tree.svg"
    alt="Shape Expression tree"
/>

---
layout: big-diagram
---

# 3️⃣ 3<sup>rd</sup> superstep -- Aggregate Messages

::big::

<img
    class="mx-auto"
    src="/public/img/agg_messages_third.svg" 
    alt="PSchema algorithm trace"
/>

::small::

<img
    class="mx-auto"
    src="/public/img/third_tree.svg"
    alt="Shape Expression tree"
/>

---
layout: big-diagram
---

# 3️⃣ 3<sup>rd</sup> superstep -- Vertex Program

::big::

<img
    class="mx-auto"
    src="/public/img/vertex_program_third.svg" 
    alt="PSchema algorithm trace"
/>

::small::

<img
    class="mx-auto"
    src="/public/img/third_tree.svg"
    alt="Shape Expression tree"
/>

---

# 🏁 Resulting _subgraph_

<img
    class="mx-auto"
    src="/public/img/result.svg" 
    alt="PSchema algorithm trace"
    width="750"
/>

---
layout: quote
---

<h2> 🎓 Lemma. Convergence of the PSchema algorithm. </h2> 

<h4 class="mt-2">
Given a Shape Expression tree <em>𝐓</em> and a
Knowledge Graph <em>𝐆</em>, let <em>𝒉</em> denote the height of <em>𝐓</em>; then the PSchema algorithm is going to converge
in <em>𝒉</em> supersteps. This is, the algorithm is going to validate all the Shapes of <em>𝐓</em> in <em>𝒉</em> supersteps.
</h4>

<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+Math&display=swap');
em {
    font-family: 'Noto Sans Math', sans-serif;
    font-style: normal;
    font-weight: 400;
}
</style>

---
layout: center
---

# 🚄 Optimizations

- Dictionary encoding
- Efficient caching
- Columnar storage


---
layout: section
---

# 🧪 Experiments

- ## 🧬 Uniprot
- ## 📖 Wikidata

---

# 📊 Results

<br />

## 👎 No optimization enabled

|   Shape Expression   | Initial triples | Resulting triples | Time (s) | Memory (GB) |
|:--------------------:|:---------------:|:-----------------:|:--------:|:-----------:|
| `protein`              | 7,346,129       | 226,241           | 23.35    | 6.74        |
| `subcellular_location` | 7,346,129       | 1,084,151         | 57.56    | 6.04        |

## 👍 All optimizations enabled

|   Shape Expression   | Initial triples | Resulting triples | Time (s) | Memory (GB) |
|:--------------------:|:---------------:|:-----------------:|:--------:|:-----------:|
| `protein`              | 7,346,129       | 226,241           | 14.58    | 3.87        |
| `subcellular_location` | 7,346,129       | 1,084,151         | 37.76    | 3.75        |


---
layout: three-images
---

::left::

<img
    class="mx-auto h-auto w-full"
    src="/public/img/n.svg" 
    alt="Row-oriented storage"
/>

::top::

<img
    class="mx-auto h-auto w-full"
    src="/public/img/depth.svg" 
    alt="Row-oriented storage"
/>

::bottom::

<img
    class="mx-auto h-full w-full"
    src="/public/img/breadth.svg" 
    alt="Row-oriented storage"
/>

---
layout: section
---

# 🔚 Conclusions

---
layout: diagram-footer
---

<h1> 🔧 The tool<sup>1</sup> </h1>

::content::

<img
    class="m-auto h-full w-full"
    src="/public/img/conclusions.svg" 
    alt="Conclusions"
/>

::footer::

<Footnotes separator>
    <Footnote :number=1>
    <a href="https://github.com/weso/pschema"> PSchema </a> is implemented in Rust.
    </Footnote>
</Footnotes>

---
layout: center
---

# 🔮 Future Work

- 💻 Processing large datasets in computers with limited resources
- 🔁 Including support for Recursive Shapes
- ✂️ Early prune strategy

---
layout: end
---

---
layout: center
---

| **Feature**                      | **Supported** | **PSchema Representation** |
|----------------------------------|:-------------:|--------|
| Triple constraints               | ✅ | TripleConstraint, ShapeAnd, ShapeOr |
| Cardinality                      | ✅ |    Cardinality      |
| Labels, descriptions and aliases | ❌ | ---  |
| Value sets                       | ✅ |     ShapeOr      |
| Built-in DataTypes               | ✅ | ShapeLiteral |
| Facets                           | ❌ | --- |
| Qualifiers                       | ❌ | --- |
| References                       | ✅ | ShapeReference |
| Ranks                            | ❌ | --- |
| SiteLinks                        | ❌ | --- |

---
layout: center
---

# 💾 Import and export formats

| **Feature** | **Supported** | **Comments** |
|-------|:-----------:|--------|
|    DuckDB Import | ✅ |     ---     |
|    DuckDB Export  | 🕒 | Even if it's planned, we are waiting for the `pola-rs` release |
|    Parquet Import  | ❌ |     ---     |
|    Parquet Export  | ✅ |     ---     |
|    NTriples Import  | ✅  |    ---      |
|    NTriples Export  | ✅ |     ---     |

---
layout: two-cols-bottom
---

# 🗃️ How is the dataset stored?

::left::

<h2> 📦 Row-oriented </h2>

- Each **row** contains _heterogeneous_ data
- **Row stores** for _transactional_ workloads
- JSON introduces **boilerplate**.

<img
    class="mx-auto"
    src="/public/img/row.svg" 
    alt="Row-oriented storage"
/>

::right::

<h2> 📦 Column-oriented<sup>1</sup> </h2>

- **Columns** contains _homogeneous_ data
- **Columnar stores** for _analytical_ workloads
- DuckDB is a column-oriented database

<img
    class="mx-auto"
    src="/public/img/col.svg" 
    alt="Columnar-oriented storage"
    height="300px"
/>

::bottom::

<Footnotes separator>
    <Footnote :number=1>
    <a href="https://github.com/weso/wd2duckdb"> wd2duckdb </a> aims for giving support to the column-oriented database DuckDB for storing Wikidata dumps.
    </Footnote>
</Footnotes>


---
layout: two-cols-bottom
---

# 🔁 Move-to-Front Coding and Caching

::left::

The **Move-to-Front Coding** is a **lossless** _transformation algorithm_ which processes a sequence of symbols and produces a sequence of integers. The **idea** is that the most recently used symbols are more likely to be used again in the near future. Hence, we can maintain a list of those and when one is used, it is moved to the front of the list.

1. This allows us to **exploit the locality**
2. **Most frequent** items require **fewer bits**
3. **Cache** misses are **less likely** to happen

👀 `Integers` _usually_ require **less bytes** to be stored than `Strings`: _cache efficiency_.

<style>
.slidev-layout ol {
    list-style: none;
    padding: 0;
    margin: -1em;
}   

ol li {
    padding-left: 1rem;
    text-indent: -0.75rem;
}

ol li::before {
    content: "😲 ";
}

ol li:nth-child(2)::before {
    content: '🤏 ';
}

ol li:nth-child(3)::before {
    content: '😎 ';
}
</style>

::right::

#### i.e Encoding<sup>1</sup> the string _panama_

| **input** |  **output**  | **list**                   |
|-----------|--------------|----------------------------|
| p         | 15           | abcdefghijklmnopqrstuvwxyz |
| a         | 15 1         | pabcdefghijklmnoqrstuvwxyz |
| n         | 15 1 14      | apbcdefghijklmnoqrstuvwxyz |
| a         | 15 1 14 1    | napbcdefghijklmoqrstuvwxyz |
| m         | 15 1 14 1 14 | anpbcdefghijklmoqrstuvwxyz |
| a         | 15 1 14 1 14 | manpbcdefghijkloqrstuvwxyz |

<style>
h4 {
    margin-left: 1em;
}
table {
    margin-left: 1em;
}
</style>

::bottom::

<Footnotes separator>
    <Footnote :number=1>
        <a href="https://www.geeksforgeeks.org/move-front-data-transform-algorithm/"> https://www.geeksforgeeks.org/move-front-data-transform-algorithm/ </a>
    </Footnote>
</Footnotes>

<style>
ul li::before {
    content: "";
}
</style>


---
layout: center
---

# 🔗 Links

- [wd2duckdb](https://github.com/weso/wd2duckdb)
- [pregel-rs](https://github.com/weso/pregel-rs)
- [pschema-rs](https://github.com/weso/pschema-rs)