# Nomeados ao Oscar

Contém a base de indicados ao Oscar em formato SQL para treinar comandos CRUD. 

Abaixo, algumas atividades para trabalharmos.

--- 

* Atualize os registros da tabela com os dados do Oscar 2025

--- Feito

* Qual o **total** de registros na tabela indicados?

--- 11004 Documentos

* Qual o número de indicações por categoria agrupadas por categoria?

R: 23

Q: 
```js
db.indicados_ao_oscar.aggregate([
  {
    $group: {
      _id: "$categoria",  
      total_indicacoes: { $sum: 1 }  
    }
  },
  {
    $sort: { total_indicacoes: -1 } 
  }
]);
```

---

* Quantas vezes Natalie Portman foi indicada ao Oscar?

R: 3 vezes

Q:
```js
 db.oscar.find({"Nome": "Natalie Portman"}).countDocuments()
```
---

* Quantos Oscars Natalie Portman ganhou?
  
R: 1 vez ganhadora do Oscar
Q: 
```js
db.Registros.countDocuments({
nome_do_indicado:"Natalie Portman",
vencedor:"true"
})
```
--- 

* Quantas vezes Viola Davis foi indicada ao Oscar?
  R: 4 vezes indicada ao Oscar
  Q:
```js
db.Registros.countDocuments({
nome_do_indicado:"Viola Davis",
})
```
---

* Quantos Oscars Viola Davis ganhou?
 R: 1 vez ganhadora do Oscar
  Q:
```js
db.Registros.countDocuments({
nome_do_indicado:"Viola Davis",
vencedor:"true"
})
```
---

* Amy Adams já ganhou algum Oscar?
R: 0 vezes ganhadora do Oscar
Q: 
```js
db.Registros.countDocuments({
nome_do_indicado:"Amy Adams",
vencedor:"true"
})
```
---

* Quais os atores/atrizes que foram indicados mais de uma vez?
  Q:
  ```js
  db.Registros.aggregate([
  { $group: { _id: "$nome_do_indicado", total_indicacoes: { $sum: 1 } } },
  { $match: { total_indicacoes: { $gt: 1 } } }
])
---
Q:
```js
db.Registros.aggregate([
  { $group: { _id: "$nome_do_indicado", total_indicacoes: { $sum: 1 } } },
  { $match: { total_indicacoes: { $gt: 1 } } },
  { $count: "quantidade_atores_mais_uma_indicacao" }
])
```

* A série de filmes Toy Story ganhou Oscars em quais anos?
R: 2 vezes, uma em 2011 e uma em 2020
Q:
```js
db.Registros.find({
nome_do_filme:/Toy Story/,
categoria: "ANIMATED FEATURE FILM",
vencedor:"true"
```
---

* A partir de que ano que a categoria "Actress" deixa de existir?
  R: Em 1928
  Q: 
```js
db.Registros.find({
categoria: "ACTRESS" 
}).sort({
ano_cerimonia: 1
}).limit(1)
---
```
* Quem ganhou o primeiro Oscar para Melhor Atriz? Em que ano?
* R: Janet Gaynor
* Q:
```js
db.Registros.find({
categoria: "ACTRESS" ,
vencedor: "true"
}).sort({
cerimonia: 1
})
```
---

* Na campo "Vencedor", altere todos os valores com "true" para 1 e todos os valores "false" para 0.
  R: Feito
Q: 
```js
db.Registros.updateMany({
    vencedor: "true"
}, {
    $set: {
        vencedor: 1
    },
});
db.Registros.updateMany({
vencedor : "false"
}, {
    $set:{
         vencedor: 0
    },
})
```
---

* Em qual edição do Oscar "Crash" concorreu ao Oscar?
  R: 2006
 Q: 
```js
db.Registros.find({
nome_do_filme:"Crash"
})
```
---
* O filme Central do Brasil aparece no Oscar?
R: Não
Q: 

  ```js
db.Registros.find({
nome_do_filme:"Central do Brasil"
})
---

* Inclua no banco 3 filmes que nunca foram nem nomeados ao Oscar, mas que merecem ser. 
R: Ready Number One, Home on the Rage
Q: 
````js
db.Registros.insertMany([
  {
 "_id": 01,
  "id_Registro": 11006,
  "ano_filmagem": 2017,
  "ano_cerimonia": 2026,
  "cerimonia": 98,
  "categoria": "Best Picture",
  "nome_do_indicado": " Steven Spielberg",
  "nome_do_filme": "Ready Number One",
  "vencedor": 1
},
{
  "_id": 02,
  "id_Registro": 11007,
  "ano_filmagem": 2004,
  "ano_cerimonia": 2026,
  "cerimonia": 98,
  "categoria": "Best Animation Picture",
  "nome_do_indicado": "will Finn",
  "nome_do_filme": "Home on the Range",
  "vencedor": 0
},
{
"_id" : 03,
"id_Registro": 11008,
  "ano_filmagem": 2006,
  "ano_cerimonia": 2026,
  "cerimonia": 98,
  "categoria": "",
  "nome_do_indicado": "Alex Winter",
  "nome_do_filme": "Ben 10: Race Against Time",
  "vencedor": 0
}
])
````
---

* Denzel Washington já ganhou algum Oscar?
R: Ganhou duas vezes
Q: 
````js
db.Registros.countDocuments({
nome_do_indicado: "Denzel Washington",
vencedor : 1
````
---

* Quais os filmes que ganharam o Oscar de Melhor Filme?
R:{ nome_do_filme: 'The Lost Weekend' },
  { nome_do_filme: 'Going My Way' },
  { nome_do_filme: 'The Best Years of Our Lives' },
  { nome_do_filme: "Gentleman's Agreement" },
  { nome_do_filme: "All the King's Men" },
  { nome_do_filme: 'Hamlet' },
  { nome_do_filme: 'All about Eve' },
  { nome_do_filme: 'An American in Paris' },
  { nome_do_filme: 'From Here to Eternity' },
  { nome_do_filme: 'On the Waterfront' },
  { nome_do_filme: 'The Greatest Show on Earth' },
  { nome_do_filme: 'Marty' },
  { nome_do_filme: 'Gigi' },
  { nome_do_filme: 'The Bridge on the River Kwai' },
  { nome_do_filme: 'Around the World in 80 Days' },
  { nome_do_filme: 'West Side Story' },
  { nome_do_filme: 'Ben-Hur' },
  { nome_do_filme: 'The Apartment' }
Q:
````js
db.Registros.find(
  {
    categoria: "BEST MOTION PICTURE", 
    vencedor: 1 
  },
  {
    nome_do_filme: 1, 
    _id: 0 
  }
).toArray()
````
---

* Sidney Poitier foi o primeiro ator negro a ser indicado ao Oscar. Em que ano ele foi indicado? Por qual filme?
R: Em 1964 Pelo filme Lilies of the Field, e em 1959 pelo filme The Defiant Ones
Q:
````js
db.Registros.find({
nome_do_indicado: "Sidney Poitier"
})
````
---

* Quais os filmes que ganharam o Oscar de Melhor Filme e Melhor Diretor na mesma cerimonia?
R:
_id: {
    cerimonia: 28,
    filme: 'Marty'
  },
  categorias: [
    'BEST MOTION PICTURE',
    'DIRECTING'
  ]
}
{
  _id: {
    cerimonia: 30,
    filme: 'The Bridge on the River Kwai'
  },
  categorias: [
    'BEST MOTION PICTURE',
    'DIRECTING'
  ]
}
{
  _id: {
    cerimonia: 26,
    filme: 'From Here to Eternity'
  },
  categorias: [
    'DIRECTING',
    'BEST MOTION PICTURE'
  ]
}
{
  _id: {
    cerimonia: 19,
    filme: 'The Best Years of Our Lives'
  },
  categorias: [
    'DIRECTING',
    'BEST MOTION PICTURE'
  ]
}
{...}
Q:
````js
db.Registros.aggregate(
    { $match: { categoria: { $in: ["BEST MOTION PICTURE", "DIRECTING"] }, vencedor: 1 } },
    { $group: { _id: { cerimonia: "$cerimonia", filme: "$nome_do_filme" }, categorias: { $addToSet: "$categoria" } } },
    { $match: { categorias: { $all: ["BEST MOTION PICTURE", "DIRECTING"] } } },
);
````
---
* Denzel Washington e Jamie Foxx já concorreram ao Oscar no mesmo ano?
R: Sim ja foram ganhadoras do mesmo ano
````js
db.registros.find({ nome_do_indicado: "Denzel Washington" })
db.registros.find({ nome_do_indicado: "Jamie Foxx" })
`````
