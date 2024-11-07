# E o Oscar vai para?
###
1. **Quantas vezes Natalie Portman foi indicada ao Oscar?**
```kt
> db.registros.countDocuments({nome_do_indicado: 'Natalie Portman'})
< 3 
```

###
2. **Quantos Oscars Natalie Portman ganhou?**
```kt
> db.registros.countDocuments({nome_do_indicado: 'Natalie Portman', vencedor: 'true'})
< 1
```

###
3. **Amy Adams já ganhou algum Oscar?**
```kt
> db.registros.countDocuments({nome_do_indicado: 'Amy Adams', vencedor: 'true'})
< 0
```

###
4. **A série de filmes Toy Story ganhou um Oscar em quais anos?**
```kt
> db.registros.distinct("ano_cerimonia", {nome_do_filme: /Toy Story/, vencedor: 'true'})
< [ 2011, 2020 ]
```

###
5. **A partir de que ano que a categoria "Actress" deixa de existir?**
```kt
> db.registros.find({categoria: 'ACTRESS'}, {ano_cerimonia: 1, categoria: 1, _id: 0}).sort({ano_cerimonia: -1}).limit(1)
< {
  ano_cerimonia: 1976,
  categoria: 'ACTRESS'
  }
```

###
6. **O primeiro Oscar para melhor Atriz foi para quem? Em que ano?**
```kt
> db.registros.find({categoria: 'ACTRESS', vencedor:'true'}, {ano_cerimonia: 1, categoria: 1, nome_do_indicado: 1}).limit(1)
< {
  _id: ObjectId('66ead39b088afa60aee4b4c8'),
  ano_cerimonia: 1928,
  categoria: 'ACTRESS',
  nome_do_indicado: 'Janet Gaynor'
  }
```

###
7. **Na campo "Vencedor", altere todos os valores com "Sim" para 1 e todos os valores "Não" para 0.**
```kt
> db.registros.updateMany({vencedor: "true"}, {$set: {vencedor: 1}});
< {
  acknowledged: true,
  insertedId: null,
  matchedCount: 2464,
  modifiedCount: 2464,
  upsertedCount: 0
  }
> db.registros.updateMany({vencedor: "false"}, {$set: {vencedor: 0}});
< {
  acknowledged: true,
  insertedId: null,
  matchedCount: 8423,
  modifiedCount: 8423,
  upsertedCount: 0
  }
```
###
8. **Em qual edição do Oscar "Crash" concorreu ao Oscar?**
```kt
> db.registros.distinct('cerimonia', {nome_do_filme: 'Crash'})
< [ 78 ]
```

###
9. **Bom... dê um Oscar para um filme que merece muito, mas não ganhou.**
```kt
> db.registros.updateOne({vencedor: 1, ano_cerimonia: 2021}, {$set: {vencedor: 0}})
< {
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
  }
> db.registros.updateOne({nome_do_indicado: 'Bert Hamelinck and Sacha Ben Harroche, Producers', ano_cerimonia: 2021}, {$set: {vencedor: 1}})
< {
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
  }
```

###
10. **O filme Central do Brasil aparece no Oscar?**
```kt
> db.registros.countDocuments({nome_do_filme: 'Central Station'})
< 2
```

###
11. **Inclua no banco 3 filmes que nunca foram nem nomeados ao Oscar, mas que merecem ser.**
```kt
> db.registros.insertMany([{
  id_registro: 45964,
  ano_filmagem: 2011,
  ano_cerimonia: 2012,
  cerimonia: 84,
  categoria: 'BEST PICTURE',
  nome_do_indicado: 'Greg Berlanti and Donald De Line, Producers',
  nome_do_filme: 'Green Lantern',
  vencedor: 0
    },{
  id_registro: 45964,
  ano_filmagem: 2011,
  ano_cerimonia: 2012,
  cerimonia: 84,
  categoria: 'DIRECTING',
  nome_do_indicado: 'Martin Campbell',
  nome_do_filme: 'Green Lantern',
    vencedor: 0
    },{
  id_registro: 45964,
  ano_filmagem: 2011,
  ano_cerimonia: 2012,
  cerimonia: 84,
  categoria: 'CINEMATOGRAPHY',
  nome_do_indicado: 'Dion Beebe',
  nome_do_filme: 'Green Lantern',
  vencedor: 0
    }])
< {
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('66eaeb4b81c952e0095bc597'),
    '1': ObjectId('66eaeb4b81c952e0095bc598'),
    '2': ObjectId('66eaeb4b81c952e0095bc599')
  }
  }
```

###
12. **Pensando no ano em que você nasceu: Qual foi o Oscar de melhor filme, Melhor Atriz e Melhor Diretor?**
```kt
> db.registros.find({ano_cerimonia: 2003, vencedor: 1, categoria: { "$in": [/LEADING ROLE/, /PICTURE/]}}, {categoria: 1, nome_do_indicado: 1, nome_do_filme: 1, _id: 0})
< {
  categoria: 'ACTOR IN A LEADING ROLE',
  nome_do_indicado: 'Adrien Brody',
  nome_do_filme: 'The Pianist'
  }
  {
  categoria: 'ACTRESS IN A LEADING ROLE',
  nome_do_indicado: 'Nicole Kidman',
  nome_do_filme: 'The Hours'
  }
  {
  categoria: 'BEST PICTURE',
  nome_do_indicado: 'Martin Richards, Producer',
  nome_do_filme: 'Chicago'
  }
```



