---
layout: post
title: "Annotating train data with taxnomomies"
date: 2021-05-03 18:00:00 -0000
categories: NLP ML
---

I want to assign products to shop departments they can be found in.

Each product belongs to one and only one product category (e.g., ??). Product categories are organized into a taxonomy, there is a hierarchy of product categories. 

We assume that all products that (directly) belong to the same category, can be found in the same shop department.

Given that assumption, we can first map product categories to corresponding departments and using that mapping solve our initial problem: assign product to departments.

