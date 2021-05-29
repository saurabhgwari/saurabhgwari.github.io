---
layout: post
title:  "Handling missing data with Alteryx"
date:   2021-05-21 09:29:20 +0700
tags: [Alteryx]
image: "/handle-missing-data/issue.png"
---

Received a request from Client to fill missing values in their dataset containing **11 years** of data.
1. `Date` and `Time` fields required new rows to be created for missing values
2. `DataPointers` fileds required last time value to be inserted as missing values  

<figure>
    <img src="/handle-missing-data/issue.png" alt="Snippet of Dataset">
    <figcaption>Sample from Data highlighting requirement</figcaption>
</figure>

I picked up _Alteryx_ for this job since it easily allows news rows to be created with *Generate rows* tool. Post which I used *Multi-Row Formula* tool to handle datapointers missing values. Here is my **solution** - 

<figure>
    <img src="/handle-missing-data/workflow.jpg" alt="Alteryx workflow">
    <figcaption>Sample from Data highlighting requirement</figcaption>
</figure>

This task would have taken about 4-5 days, if done manually, however using workflow it was automated to **<12 secs** with scope of scaling this solution to more datasets.