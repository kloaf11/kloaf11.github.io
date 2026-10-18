---
title: Refinement Plan and Code Review
description: Refinement plan and code review for a Java zoo monitoring program, covering software design, algorithms and data structures, and databases.
---

## Introduction

This refinement plan was part of the February 2021 CS-499 portfolio. It is written as a plan. The enhancement pages describe the work that was completed.

### Software engineering and design

I will take my zoo monitoring program from IT-145 and use it for the software engineering and design category. Right now, the code uses rudimentary files and offers few ways to check on the animals and habitats. I will enhance the program with data files for more animals and habitats, an employee section, a zoo transfer system, and improved code structure so it is more modular and efficient.

### Algorithms and data structures

I will work with the same artifact as above, my zoo monitoring program from IT-145. For this category I will use a more robust structure for reading the data that is kept. Currently it uses only .txt files and reads them line by line for the output. For this step I will add more data per file and retrieve data from a SQL database instead of guessing the file layout and the amounts. The output will also vary based on the data in the result set, instead of showing all the data and reacting to rudimentary prompts. This part also ties into the next part of the enhancements.

### Databases

This artifact will, in theory, be made from scratch, but you can consider it an improvement on the data held in the zoo monitoring program, which at the start of this project was text files. I will create a SQL Server database with Microsoft SQL Server, and I will create the relational database for the zoo monitoring program to access. The object types will be tables that the program reads from and returns as a result set.

## Code review

<div class="video-wrap">
  <iframe src="https://www.youtube.com/embed/asyU3Cr6Yto" title="Code review video for the zoo monitoring project" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

{% include portfolio-nav.html current="review" %}
