---
date: 2021-Aug-21
tags: ["glossary", "ddd", "ddd_relationship"]
---

# Anti-Corruption Layer
Downstream bounded context membuat sebuah layer yang bertujuan untuk mentranslasi data atau object yang datang dari upstream bounded context, dan memastikan data/object data tersebut compatible dengan internal model.

- Downstream don't want to be influenced by the upstream
- Downstream will consume the upstream's model and translate the data into its own structure