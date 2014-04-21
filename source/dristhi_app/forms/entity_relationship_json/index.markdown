---
layout: page
title: "entity_relationship.json"
---

# entity_relationship.json
Another important component of Dristhi forms is entity_relationship.json. There is one entity_relationship.json per application (in this case Dristhi). As the name suggests, it defines the relationship between the various entities of Dristhi app. The entities that are part of the app as of now are *eligible_couple, mother and child*. Dristhi's entity_relationship.json is below:
```json
[
    {
        "parent": "eligible_couple",
        "child": "mother",
        "field": "wife",
        "kind": "one_to_one",
        "from": "eligible_couple.id",
        "to": "mother.ecCaseId"
    },
    {
        "parent": "mother",
        "child": "child",
        "field": "children",
        "kind": "one_to_many",
        "from": "mother.id",
        "to": "child.motherCaseId"
    }
]
```
Each entry in list defines a relation. We have two relationships in Dristhi, one_to_one between eligible_couple and mother and one_to_many between mother and child.
Explanation for the fields below:  
**parent**  
This specifies the parent entity in the given relation.  
**child**  
This specifies the child entity in the given relation.  
**field**  
This specifies the field on parent entity which allows us to load child entity for that parent. This was added to support the cases where entities are stored on Document based Databases like CouchDB. This is currently not used or implemented in the app.  
**kind**  
This specifies the type of relationship. Currently supported valyes are *one_to_one* and *one_to_many*.  
**from** and **to**  
Together these two values specify how to load parent and child entities from a relational database.  
