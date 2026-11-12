## Parte 14 — Exercícios práticos

Utilize `categoria` e `produto` criadas no início.

### Nível 1 — INNER JOIN

1. Apresente o nome de cada produto e sua categoria.
2. Apresente nome, preço e categoria, em ordem decrescente de preço.
3. Consulte somente os produtos de Informática.
4. Apresente produtos com preço superior a R$ 500 e suas categorias.
5. Apresente produtos de Informática ou Móveis, em ordem alfabética.

### Nível 2 — LEFT JOIN

6. Liste todas as categorias, incluindo as que não possuem produtos.
7. Identifique as categorias sem produtos.
8. Liste todas as categorias e associe somente produtos com preço superior a R$ 100, preservando categorias sem correspondências.

### Nível 3 — Comparações

9. Execute uma consulta com `INNER JOIN` e outra com `LEFT JOIN`; compare os resultados.
10. Reescreva uma consulta com `LEFT JOIN` usando `RIGHT JOIN` e preserve o resultado.
11. Execute um `CROSS JOIN` e explique a quantidade de linhas.

### Nível 4 — Modelagem e consulta

12. Crie `cliente` e `pedido` em um relacionamento 1:N. Cadastre três clientes e quatro pedidos, deixando um cliente sem pedidos. Escreva consultas para:

    a. mostrar os pedidos com seus clientes;
    b. mostrar todos os clientes, inclusive os sem pedidos;
    c. mostrar apenas clientes sem pedidos;
    d. ordenar pedidos por data;
    e. mostrar os pedidos de um cliente específico.

---