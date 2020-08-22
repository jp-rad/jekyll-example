---
title: PlantUML
layout: page
---

> Demo of the [jekyll-plantuml](https://rubygems.org/gems/jekyll-plantuml) gem.

Example of the [PlantUML](https://plantuml.com/)

## Sequence diagram
{% plantuml %}
Alice -> Bob: Authentication Request
Bob --> Alice: Authentication Response

Alice -> Bob: Another authentication Request
Alice <-- Bob: another authentication Response
{% endplantuml %}

## Entity Relationship diagram

{% plantuml %}
' hide the spot
hide circle

' avoid problems with angled crows feet
skinparam linetype ortho

entity "Entity01" as e01 {
  *e1_id : number <<generated>>
  --
  *name : text
  description : text
}

entity "Entity02" as e02 {
  *e2_id : number <<generated>>
  --
  *e1_id : number <<FK>>
  other_details : text
}

entity "Entity03" as e03 {
  *e3_id : number <<generated>>
  --
  e1_id : number <<FK>>
  other_details : text
}

e01 ||..o{ e02
e01 |o..o{ e03
{% endplantuml %}
