---
layout: page
title: Staples
permalink: /staples/
description: Machine Learning Intern, Jun – Sep 2024 · AI Engineer, Sep 2024 – Jan 2026
nav: false
---

<!-- Each project is a .stint section on a shared timeline line (styled in _sass/_custom.scss). -->
<div class="stints">

<section class="stint" markdown="1">
<i class="fa-solid fa-magnifying-glass stint-icon" aria-hidden="true"></i>

## Product matching

<p class="stint-dates">AI Engineer</p>

I was a member of the modeling team on the internal product matching initiative, which matches short product descriptions to items in the catalog.

I developed an evaluation framework for information retrieval systems, which scores models on Success@K, Recall@K, Coverage@K, and MRR across three use cases: cross-reference, competitor, and internal. I designed it around three modular pillars: the retriever to score, the dataset to score it on, and the metrics to score it with. Any one of them can be swapped while the other two stay fixed, so every comparison is like for like. The framework replaced manual spot checks and gave us the reporting necessary to decide which models to keep developing and which to put into production.

I also contributed to fine-tuning transformer embedding models on internal datasets. I implemented a custom PyTorch training loop, a multiple-negatives ranking loss, semi-hard negatives mined with FAISS, experiment tracking on MLflow, and a 433k-pair training set by merging cross-reference and competitor match data. The retriever reached **87.8% Success@10** and **75.8% Coverage@10**.

To implement CI/CD for our inference logic, I created Databricks Asset Bundles and deployed them to our staged environments using Azure Pipelines.

I helped lead an evaluation of whether product images could improve matching, by fusing text retrieval with CLIP image retrieval through reciprocal rank fusion. Sparse image coverage in the catalog and weak CLIP matches turned out to be the main limits, and I wrote documentation on rank fusion and multimodal retrieval.

<p class="stint-tags">PyTorch · sentence-transformers · FAISS · CLIP · MLflow · Databricks Asset Bundles · Azure Pipelines · uv</p>
</section>

<section class="stint" markdown="1">
<i class="fa-solid fa-robot stint-icon" aria-hidden="true"></i>

## HR handbook assistant

<p class="stint-dates">AI Engineer</p>

I built an assistant that answers employee questions about the employee handbook in plain language. It's an agentic retrieval-augmented generation (RAG) system: a ReAct agent built with LangGraph on Azure OpenAI, which looks up the relevant parts of the handbook before it answers.

<p class="stint-tags">LangGraph · Azure OpenAI · RAG · ReAct</p>
</section>

<section class="stint" markdown="1">
<i class="fa-solid fa-gears stint-icon" aria-hidden="true"></i>

## Pricing

<p class="stint-dates">Machine Learning Intern</p>

I refactored an existing ML pricing pipeline down to 91.5% less code, and restructured it so models could be swapped while it was running. I shipped it with a full test suite and documentation.

I also worked on the pricing models themselves. With my mentor, I derived a closed-form formula for the price that maximizes margin given a product's price elasticity. I also prototyped an LSTM that forecasts a product's sales volume as a function of price from its price and volume history, reaching a 22.3% mean absolute percentage error.

The work earned me a full-time return offer, and I joined Staples as an AI engineer that September.

</section>

</div>
