---
layout: post
title: "The Quest for a Native Neuro-Symbolic Database: Introducing MEB"
date: 2026-01-09
categories: [AI, Database, Technology]
tags: [neuro-symbolic-ai, database, meb, knowledge-graphs, vector-database, hybrid-systems]
---

# The Quest for a Native Neuro-Symbolic Database: Introducing MEB

In the rapidly evolving landscape of AI, we are witnessing the rise of **Graph RAG** and **Neuro-Symbolic** systems. These architectures promise something profound: the ability to combine the cold, hard logic of knowledge graphs with the fluid, semantic intuition of vector embeddings.

When we began our journey into this space, we had a clear vision of what we needed. We wanted a database that could reason like a person but scale like a machine.

## The Inspiration and the Friction

Any discussion about neuro-symbolic databases must acknowledge **CozoDB**. It is a magnificent piece of engineering—a pioneer that proved you could successfully bridge the gap between Datalog and vectors. For many, it is the gold standard.

However, as a team deeply invested in the **Go ecosystem**, we encountered a practical friction point: **CGO**.

CozoDB is powerful, but its reliance on CGO and external libraries made our deployment pipelines increasingly complex. When you are targeting a wide range of environments—from ephemeral **Cloud Run** instances to constrained **IoT edge devices**—the "Single Binary" promise of Go becomes more than just a preference; it becomes a necessity. We wanted the simplicity of `go build` resulting in a portable, native binary that runs everywhere without shared library headaches.

## Why We Built MEB: The Pure Go Philosophy

MEB (Mangle Extension for Badger) was born from a simple question: **Can we build a massively performant neuro-symbolic engine in Pure Go?**

We believed that by carefully selecting the right building blocks, we could achieve production-grade performance without ever leaving the Go safety zone.

### 1. The Reasoning: Google Mangle

We chose **Google Mangle** for the logic layer. It brings **Datalog** to Go, allowing for the kind of recursive, structured reasoning that modern knowledge graphs require. It’s elegant, declarative, and—most importantly—written in native Go.

### 2. The Storage: BadgerDB

For the storage engine, **BadgerDB** was the only choice. Its **WiscKey** architecture, which separates keys from values, is a masterpiece of SSD optimization. It allowed us to manage a billion graph facts on disk while maintaining the high throughput needed for real-time applications.

### 3. The Semantics: Native Vector Search

The "Neuro" in Neuro-Symbolic comes from our native vector layer. We implemented **Matryoshka Representation Learning (MRL)** and **Scalar Quantization (SQ8)** to compress -dimensional embeddings into compact -byte signatures. This allows us to perform similarity searches across millions of records with sub-millisecond latency, all within the Go runtime.

## Massive Performance, Universal Portability

A common misconception is that "Pure Go" means sacrificing performance. Our benchmarks tell a different story. By leveraging the latest features in **Go 1.23**, such as new iterators and improved compiler optimizations, MEB delivers:

* **Sub-millisecond** query latency.
* **10M+ documents** indexed on a single NVMe node.
* **Seamless portability** across AMD64 and ARM64 architectures.

By avoiding CGO, we made MEB "Universal." It doesn't matter if you are deploying to an **AWS Graviton** server, a **MacBook M3**, or a **Raspberry Pi**; the binary is the same, and the performance remains massive.

## A Tool for the Modern AI Stack

MEB is an embedded database. It lives inside your application, eliminating the "database tax" of network latency and complex infrastructure management.

* **For RAG:** Isolate knowledge contexts using our multi-tenant "Graph" support.
* **For Code Intelligence:** Traverse complex call graphs using Datalog.
* **For Edge AI:** Perform local reasoning on-device without a cloud connection.

## Conclusion

We didn't build MEB because the world lacked powerful databases. We built it because we believed that the future of Neuro-Symbolic AI should be **accessible**, **portable**, and **native**.

MEB is our contribution to the Go community—a tool for those who want to build the next generation of intelligent applications without the friction of complex deployments.

**Check out the project on GitHub:** [https://github.com/duynguyendang/meb](https://github.com/duynguyendang/meb)