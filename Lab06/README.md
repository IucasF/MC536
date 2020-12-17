# Modelo para Apresentação do Lab06 - Cypher e FAERS

Estrutura de pastas:

~~~
├── README.md  <- arquivo apresentando a tarefa
~~~

## Tarefa de Cypher e o FDA Adverse Event Reporting System (FAERS)

## Exercício 1

Escreva uma sentença em Cypher que crie o medicamento de nome `Metamizole`, código no DrugBank `DB04817`.

### Resolução
~~~cypher
CREATE (:Drug {drugbank: "DB04817", name:"Metamizole"})
~~~

## Exercício 2

Considerando que a `Dipyrone` e `Metamizole` são o mesmo medicamento com nomes diferentes, crie uma aresta com o rótulo `:SameAs` que ligue os dois.

### Resolução
~~~cypher
MATCH (d1 {name:"Dipyrone"})
MATCH (d2 {name:"Metamizole"})
CREATE (d1)-[:SameAs]->(d2)
~~~

## Exercício 3

Use o `DELETE` para excluir o relacionamento que você criou (apenas ele).

### Resolução
~~~cypher
MATCH (d1 {name:"Dipyrone"})-[e]->(d2 {name:"Metamizole"})
delete e
~~~

## Exercício 4

Faça a projeção em relação a Patologia, ou seja, conecte patologias que são tratadas pela mesma droga.

### Resolução
~~~cypher
MATCH (p1:Pathology)-[a]->(d:Drug)<-[b]-(p2:Pathology)
WHERE a.weight > 20 AND b.weight > 20
MERGE (d1)<-[r:Relates]->(d2)
ON CREATE SET r.weight=1
ON MATCH SET r.weight=r.weight+1
~~~

## Exercício 5

Construa um grafo ligando os medicamentos aos efeitos colaterais (com pesos associados) a partir dos registros das pessoas, ou seja, se uma pessoa usa um medicamento e ela teve um efeito colateral, o medicamento deve ser ligado ao efeito colateral.

### Resolução
~~~cypher
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/drug-use.csv' AS line
CREATE (:Pessoa { idperson: line.idperson})
CREATE INDEX ON :Pessoa(idperson)

LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/drug.csv' AS line
CREATE (:Drug {code: line.code, name: line.name})
CREATE INDEX ON :Drug(code)

LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/pathology.csv' AS line
CREATE (:Pathology { code: line.code, name: line.name})
CREATE INDEX ON :Pathology(code)

LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/drug-use.csv' AS line
MATCH (d:Drug {code: line.codedrug})
MATCH (p:Pessoa {code: line.idperson})
MERGE (d)-[t:Usou]->(p)
ON CREATE SET t.weight=1
ON MATCH SET t.weight=t.weight+1

LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/sideeffect.csv' AS line
MATCH (d:Drug {Pathology: line.codePathology})
MATCH (p:Pessoa {code: line.idperson})
MERGE (d)-[t:Pegou]->(p)
ON CREATE SET t.weight=1
ON MATCH SET t.weight=t.weight+1

MATCH (d:Drug)-[a]->(p:Pessoa)<-[b]-(o:Pathology)
MERGE (d)<-[r:Relates]->(o)
ON CREATE SET r.weight=1
ON MATCH SET r.weight=r.weight+1
~~~

## Exercício 6

Que tipo de análise interessante pode ser feita com esse grafo?

Selecionar apenas relações de efeito colateral que ocorreu com mais de 10 pessoas.

### Resolução
~~~cypher
MATCH (d:Drug)<-[r :Relates]-> (p:Pathology)
RETURN d,p
WHERE r.weight>10
~~~