# Modelo para Apresentação do Lab08 - PubChem e DRON com XQuery/XPath

Estrutura de pastas:

~~~
├── README.md  <- arquivo apresentando a tarefa
~~~

## Tarefas com Publicações

## Questão 1
Construa uma comando SELECT que retorne dados equivalentes a este XPath
~~~xquery
//individuo[idade>20]/@nome
~~~

### Resolução
~~~xquery
SELECT nome FROM INDIVIDUO I
WHERE I.idade > 20;
~~~

## Questão 2
Qual a outra maneira de escrever esta query sem o where?

~~~xquery
let $fichariodoc := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/xml/fichario.xml')
 
for $i in ($fichariodoc//individuo)
where $i[idade>17]
return {data($i/@nome)}
~~~
### Resolução
~~~xquery
let $fichariodoc := doc('http://www.ic.unicamp.br/~santanch/teaching/db/xml/fichario.xml')

return $fichariodoc//individuo[nome="@nome"]
~~~

## Questão 3
Escreva uma consulta SQL equivalente ao XQuery:
~~~xquery
let $fichariodoc := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/xml/fichario.xml')

for $i in ($fichariodoc//individuo)
where $i[idade>17]
return {data($i/@nome)}
~~~

### Resolução
~~~sql
SELECT individuo FROM fichariodo
WHERE nome=nome
~~~

## Questão 4
Retorne quantas publicações são posteriores ao ano de 2011.

### Resolução
~~~xquery
let $fichariodoc := doc('http://www.ic.unicamp.br/~santanch/teaching/db/xml/fichario.xml')

for @@3@@fichariodoc//individuo)
where @@4@@i/@idade
return <maior>{data($i/@idade)}</maior>
~~~

## Questão 5
Retorne a categoria cujo `<label>` em inglês seja 'e-Science Domain'.

### Resolução
~~~xquery
(escreva aqui a resolução em XQuery)
~~~

## Questão 6
Retorne as publicações associadas à categoria cujo `<label>` em inglês seja 'e-Science Domain'. A associação entre o label e a key da categoria deve ser feita na consulta.

### Resolução
~~~xquery
(escreva aqui a resolução em XQuery)
~~~
