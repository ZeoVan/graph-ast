# Graph-Ast

A tool to generate the graph representation of the source code based on the paper: [Learning to Represent Program with Graph, ICLR 2018](https://arxiv.org/abs/1711.00740)

## Installation

The backbone of this tool is the Abstract Syntax Tree (AST). The AST will be generated using the f-ast tool: [fAST: Flattening Abstract Syntax Trees for Efficiency, ICSE 2019](https://oro.open.ac.uk/59268/1/main.pdf). The tool supports any ANTLR4 grammar of over 170 different types of programming languages. 

Some benefits of using the f-ast:

- f-ast leverages [protobuf](https://github.com/protocolbuffers/protobuf) to store the AST and make the parsing much faster than the others.
- f-ast is built based on [srcml](https://www.srcml.org/) and [srcSlice](https://github.com/srcML/srcSlice). That is, it can incorporate the data flow information, such as [the use-def chain](https://en.wikipedia.org/wiki/Use-define_chain) (t into the AST. The use-def chain is a critical information to generate the graph-ast.

A runnable docker image of the tool can be pulled by using this command:

```bash
  $ docker pull yijun/fast:latest
```

## Example usages:

To generate an AST representation of a file.

```bash
  $ cd sample_files
  $ docker run -v $(pwd):/e -it yijun/fast -p Test.java Test.pb
```

The Test.pb file is the AST representation under the protobuf format. For example on how to read and traverse the tree, see this link.

Since the goal of this tool is to generate the graph representation of the source code, the next step is to run:

```python
  $  python3 generate_graph Test.pb Test.txt
```

The Test.txt is a graph representation with the format: source_id, source_node_type edge_type sink_id, sink_node_type.
For example, the edge:
```
22,3 1 21,4
```
means that the node with id 22 connects to the node with id 21 via the edge with id 1. Also, the node with id 22 has the type of 3, the node with id 21 has the type of 4.

For the list of node types, see [this](https://github.com/bdqnghi/graph-ast/blob/master/srcml_node_types.tsv).
For the list of edge types, see [this](https://github.com/bdqnghi/graph-ast/blob/master/edge_types.tsv).
