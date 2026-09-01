# Atividade Assíncrona — Integridade de Dados em Bancos de Dados Relacionais

Responder em qualquer formato, commitar no Github e publicar o link no classroom.


## 1. Contexto

Nas aulas anteriores, estudamos a evolução da modelagem de bancos de dados:

> **Mundo real → MER/DER → Modelo Relacional**

Já sabemos representar entidades, atributos e relacionamentos e transformar essa representação em tabelas, identificando **chaves primárias (PK)** e **chaves estrangeiras (FK)**.

Considere o seguinte modelo:

```text
CLIENTE
────────────────────
idCliente (PK)
nome
email

PEDIDO
────────────────────
idPedido (PK)
data
valor
idCliente (FK)
```

O modelo parece adequado. Mas surge uma nova questão:

> **O que impede que dados incorretos, contraditórios ou inconsistentes sejam armazenados nessas tabelas?**

Por exemplo:

### Tabela `CLIENTE`

| idCliente | nome   | email              |
|----------:|--------|--------------------|
| 1         | Ana    | ana@email.com      |
| 2         | Bruno  | bruno@email.com    |
| 2         | Carlos | carlos@email.com   |

Nesse caso, há dois clientes com o mesmo identificador.

### Tabela `PEDIDO`

| idPedido | data       | valor  | idCliente |
|---------:|------------|-------:|----------:|
| 101      | 30/08/2026 | 250,00 | 1         |
| 102      | 30/08/2026 | -80,00 | 2         |
| 103      | 31/08/2026 | 150,00 | 999       |

Algumas perguntas aparecem:

- Um pedido pode possuir valor negativo?
- Pode existir um pedido associado ao cliente 999 se esse cliente não existir?
- Dois clientes podem possuir o mesmo identificador?
- Um cliente pode ser cadastrado sem nome?
- Dois clientes podem possuir o mesmo e-mail?

Esses problemas estão relacionados à **integridade dos dados**.

---

## 2. Objetivo da atividade

Nesta atividade, você deverá pesquisar e compreender o conceito de **integridade de dados em bancos de dados relacionais**.

Ao final, você deverá ser capaz de explicar:

1. o que significa integridade de dados;
2. por que ela é importante;
3. quais são seus principais tipos;
4. quais problemas podem ocorrer quando a integridade não é garantida;
5. como um banco de dados pode impedir o armazenamento de dados inválidos.

> [!IMPORTANT]
> O objetivo principal desta atividade **não é aprender a sintaxe SQL**. Primeiro, queremos compreender o problema e as regras que precisam ser garantidas.

---

## 3. Parte I — Pesquisa

Pesquise em livros, materiais acadêmicos, documentação de SGBDs ou outras fontes confiáveis os conceitos a seguir.

### A. Integridade de dados

Explique com suas palavras:

> O que significa garantir a integridade dos dados em um banco de dados?

Não copie apenas uma definição. Explique o conceito como se estivesse explicando para outro estudante.

### B. Integridade de entidade

Pesquise o que é **integridade de entidade** e responda:

- O que ela procura garantir?
- Qual é a relação entre integridade de entidade e chave primária (PK)?
- Uma chave primária pode possuir valores repetidos?
- Uma chave primária pode ser nula?
- O que poderia acontecer se essas regras não fossem respeitadas?

Ao final, apresente **um exemplo próprio**.

### C. Integridade referencial

Pesquise o que é **integridade referencial** e responda:

- O que ela procura garantir?
- Qual é sua relação com a chave estrangeira (FK)?
- Uma FK pode referenciar um registro inexistente?
- O que significa um registro órfão?
- O que pode acontecer ao excluir um registro que está sendo referenciado por outra tabela?

Ao final, apresente **um exemplo próprio envolvendo duas tabelas relacionadas**.

### D. Integridade de domínio

Pesquise o que é **integridade de domínio** e responda:

- O que significa o domínio de um atributo?
- Como podemos determinar quais valores são válidos?
- O tipo de dado ajuda a controlar o domínio?
- Existem restrições que vão além do tipo de dado?

Considere a tabela:

```text
PRODUTO
────────────────────
idProduto
nome
preco
estoque
```

Determine quais valores deveriam ou não ser permitidos para os atributos `preco` e `estoque`. Justifique sua resposta.

---

## 4. Parte II — Investigando um banco problemático

Uma loja possui as seguintes tabelas:

### Tabela `CLIENTE`

| idCliente | nome   | email              |
|----------:|--------|--------------------|
| 1         | Ana    | ana@email.com      |
| 2         | Bruno  | bruno@email.com    |
| 2         | Carlos | carlos@email.com   |
| 4         | —      | maria@email.com    |
| 5         | João   | ana@email.com      |

### Tabela `PRODUTO`

| idProduto | nome    | preço  | estoque |
|----------:|---------|-------:|--------:|
| 10        | Teclado | 120,00 | 15      |
| 11        | Mouse   | -50,00 | 8       |
| 12        | Monitor | 900,00 | -3      |
| 13        | —       | 250,00 | 5       |

### Tabela `PEDIDO`

| idPedido | data       | valor   | idCliente |
|---------:|------------|--------:|----------:|
| 100      | 30/08/2026 | 250,00  | 1         |
| 101      | 30/08/2026 | -100,00 | 2         |
| 102      | 31/08/2026 | 500,00  | 99        |
| 103      | —          | 350,00  | 1         |

### Seja o responsável pelo banco

Analise os dados apresentados e identifique **pelo menos oito possíveis problemas de integridade**.

| Problema encontrado | Por que é um problema? | Tipo de integridade | Regra que deveria existir |
|----------------------|------------------------|---------------------|---------------------------|
| Ex.: Cliente 2 aparece duas vezes | Dois registros possuem o mesmo identificador | Entidade | `idCliente` não pode se repetir |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |

> [!NOTE]
> Não se limite a indicar o erro. Explique qual regra impediria que ele acontecesse.

---

## 5. Parte III — Definindo as regras antes do SQL

Considere novamente as tabelas:

```text
CLIENTE                 PRODUTO                 PEDIDO
──────────────────      ──────────────────      ──────────────────
idCliente               idProduto               idPedido
nome                    nome                    data
email                   preco                   valor
                        estoque                 idCliente
```

Você foi contratado para definir as regras que o banco deverá respeitar.

Escreva **pelo menos duas regras para cada tabela**. Continue até considerar que as três tabelas possuem regras suficientes para evitar os principais problemas identificados anteriormente.

Exemplos:

```text
R01 — Todo cliente deve possuir um identificador único.
R02 — Um pedido somente poderá referenciar um cliente existente.
R03 — …
R04 — …
```

### Classifique suas regras

Para cada regra criada, indique sua categoria:

| Código | Categoria |
|:------:|-----------|
| `[E]`  | Integridade de Entidade |
| `[R]`  | Integridade Referencial |
| `[D]`  | Integridade de Domínio |

Exemplo:

```text
[E] R01 — Todo cliente deve possuir um identificador único.
[R] R02 — Um pedido somente poderá referenciar um cliente existente.
[D] R03 — O preço de um produto não poderá ser negativo.
```

---

## 6. Parte IV — Investigação

Pesquise os seguintes termos relacionados aos bancos de dados relacionais:

- `PRIMARY KEY`
- `FOREIGN KEY`
- `NOT NULL`
- `UNIQUE`
- `CHECK`
- `DEFAULT`

Para cada termo, responda **apenas**:

1. O que ele procura garantir?
2. Dê um exemplo de situação em que seria útil.

> **Não é necessário escrever comandos SQL completos.**

---

## 7. Faça as conexões

Depois da pesquisa, relacione cada necessidade ao mecanismo que você utilizaria:

| Necessidade | Mecanismo que você utilizaria |
|------------|-------------------------------|
| Identificar cada cliente de maneira única | |
| Impedir um cliente sem nome | |
| Impedir dois clientes com o mesmo e-mail | |
| Impedir preço negativo | |
| Garantir que um pedido pertença a um cliente existente | |
| Fazer um produto iniciar com estoque zero quando nenhum valor for informado | |

Por fim, responda:

> Existe relação entre os problemas de integridade estudados e os mecanismos encontrados durante sua pesquisa? Explique.