---
layout: post
title: "Annotating train data with taxnomomies"
date: 2021-05-03 18:00:00 -0000
categories: NLP ML
---

My goal is to collect a large dataset of products labelled with shop departments they can be found in. The dataset will be used for training product classifier. 

Each product belongs to one and only one product category (e.g., ??). Product categories are organized into a taxonomy, i.e., there is a hierarchy of product categories. We assume that all products that (directly) belong to the same category, can be found in the same shop department. Given that assumption, we can first map product categories to corresponding departments and using that mapping solve our initial problem: assign product to departments.

Basically, this is a problem of mapping one (structured) vocabulary to another and is well know in the literature (see e.g., "SCHEMA - An Algorithm for Automated
Product Taxonomy Mapping in E-commerce"[1])

[1]: https://link.springer.com/content/pdf/10.1007/978-3-642-30284-8_27.pdf

