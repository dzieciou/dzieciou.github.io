---
layout: post
title: "Annotating train data with taxnomomies"
date: 2021-05-03 18:00:00 -0000
categories: NLP ML
---

My goal is to collect a large dataset of products labelled with shop departments they can be found in. The dataset will be used for training product classifier. 

Each product belongs to one and only one product category (e.g., ??). Product categories are organized into a taxonomy, i.e., there is a hierarchy of product categories. We assume that all products that (directly) belong to the same category, can be found in the same shop department. Given that assumption, we can first map product categories to corresponding departments and using that mapping solve our initial problem: assign products to departments.

Basically, this is a problem of mapping one (structured) vocabulary to another and is well know in the literature (see e.g., ["SCHEMA - An Algorithm for Automated
Product Taxonomy Mapping in E-commerce"][1]).

We have M product categories, L departments and budget N label only N of M product categories. We are looking for good labelling heuristics.

Approach
========

1. Select $N_1$ different product categories that have some products.
2. For each category draw a product.
3. Label manually each product drawn with a corresponding shop department
4. Label automatically categories of labelled products.
5. Infer automatically labels of ancestors of labelled categories.
6. Select $N_2$ (?) 

[1]: https://link.springer.com/content/pdf/10.1007/978-3-642-30284-8_27.pdf

