# Lista de Exercícios — Normalização de Bancos de Dados e SQL

### Orientações

- Utilize o SQL Editor do Supabase para os exercícios práticos.
- Apresente as justificativas dos exercícios conceituais.
- Nos exercícios de normalização, apresente o modelo lógico antes e depois da normalização.
- Utilize `CREATE TABLE`, `INSERT INTO`, `SELECT`, `UPDATE`, `DELETE` e `ALTER TABLE`, quando necessário.
- Não utilize funções de agregação (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`).
- Não é necessário utilizar `JOIN`.
- Execute cada cenário em tabelas próprias, para evitar conflitos entre os exercícios.

---

# Parte 1 — Identificação de problemas de normalização

## Exercício 1 — Identificando redundâncias

Uma instituição de ensino utiliza a seguinte tabela para armazenar informações sobre alunos e cursos:

**ALUNO_CURSO**

| id_aluno | nome_aluno | id_curso | nome_curso | coordenador |
|---|---|---|---|---|
| 1 | Ana | 10 | Sistemas de Informação | Carlos |
| 2 | Bruno | 10 | Sistemas de Informação | Carlos |
| 3 | Maria | 20 | Ciência da Computação | Fernanda |
| 4 | Pedro | 10 | Sistemas de Informação | Carlos |
| 5 | Julia | 20 | Ciência da Computação | Fernanda |

Considere que cada aluno está matriculado em apenas um curso e que cada curso possui um coordenador.

**Atividades:**

a) Identifique quais informações estão sendo armazenadas repetidamente.

b) Explique por que essa repetição pode causar problemas.

c) O que aconteceria se o coordenador do curso de Sistemas de Informação fosse alterado?

d) O que aconteceria se o único aluno de determinado curso fosse excluído?

e) Seria possível cadastrar um curso sem alunos nessa estrutura? Justifique.

f) Identifique quais entidades poderiam ser representadas separadamente.

---

## Exercício 2 — Anomalias de manipulação de dados

Considere a relação:

```text
FUNCIONARIO_DEPARTAMENTO(
    id_funcionario PK,
    nome_funcionario,
    salario,
    id_departamento,
    nome_departamento,
    gerente_departamento
)
```

Considere as seguintes regras de negócio:

- Cada funcionário pertence a um único departamento.
- Um departamento pode possuir vários funcionários.
- Cada departamento possui um nome e um gerente.
- O identificador do departamento determina seu nome e seu gerente.

**Atividades:**

a) Apresente um exemplo de anomalia de inserção.

b) Apresente um exemplo de anomalia de atualização.

c) Apresente um exemplo de anomalia de exclusão.

d) Identifique as informações que pertencem à entidade `FUNCIONARIO`.

e) Identifique as informações que pertencem à entidade `DEPARTAMENTO`.

f) Proponha uma estrutura que reduza os problemas identificados.

---

# Parte 2 — Dependências funcionais

## Exercício 3 — Identificando dependências funcionais

Considere a relação:

```text
PRODUTO(
    id_produto PK,
    nome_produto,
    preco,
    id_categoria,
    nome_categoria
)
```

**Regras de negócio:**

- Cada produto possui um identificador único.
- Cada produto pertence a uma única categoria.
- Cada categoria possui um identificador e um nome.
- Uma categoria pode possuir vários produtos.

**Atividades:**

a) Identifique a chave primária da relação.

b) Escreva as dependências funcionais existentes.

Utilize a notação:

```text
X → Y
```

c) O identificador do produto determina o nome da categoria? Explique.

d) O identificador da categoria determina o nome do produto? Justifique.

e) Existe alguma dependência transitiva envolvendo a chave primária? Identifique-a.

f) A relação apresenta problemas de normalização? Justifique.

---

## Exercício 4 — Chaves candidatas e atributos primos

Considere a relação:

```text
ALUNO(
    id_aluno,
    cpf,
    nome,
    email
)
```

**Regras de negócio:**

- `id_aluno` identifica exclusivamente cada aluno.
- `cpf` também identifica exclusivamente cada aluno.
- O CPF é obrigatório.
- O nome pode se repetir.
- O e-mail não possui garantia de unicidade.

**Atividades:**

a) Identifique todas as chaves candidatas.

b) Escolha uma chave primária.

c) Identifique os atributos primos.

d) Identifique os atributos não primos.

e) Explique por que um atributo pode ser primo sem ser a chave primária.

f) Implemente a tabela utilizando SQL, considerando `id_aluno` como chave primária e `cpf` como identificador alternativo.

---

# Parte 3 — Primeira Forma Normal (1FN)

## Exercício 5 — Aplicando a Primeira Forma Normal

Uma empresa armazena os telefones de seus clientes na seguinte tabela:

**CLIENTE**

| id_cliente | nome_cliente | telefones |
|---|---|---|
| 1 | Ana | 9999-1111, 9888-2222 |
| 2 | Bruno | 9777-3333 |
| 3 | Carlos | 9666-4444, 9555-5555, 9444-6666 |

Considere que a empresa precisa consultar, cadastrar e excluir telefones individualmente.

**Atividades:**

a) Explique por que a estrutura não atende à 1FN para o domínio considerado.

b) Identifique o atributo problemático.

c) Proponha uma estrutura normalizada.

d) Identifique as chaves primárias.

e) Identifique as chaves estrangeiras.

f) Determine a cardinalidade entre clientes e telefones.

g) Apresente o modelo lógico relacional normalizado.

### Parte prática — SQL

h) Implemente as tabelas normalizadas utilizando `CREATE TABLE`.

i) Insira os três clientes apresentados.

j) Insira todos os telefones nas respectivas tabelas.

k) Consulte todos os clientes.

l) Consulte todos os telefones pertencentes ao cliente de identificador 1.

m) Cadastre um novo telefone para o cliente de identificador 2.

n) Exclua apenas um dos telefones do cliente de identificador 3, preservando os demais.

---

## Exercício 6 — Grupos repetitivos

Uma escola desenvolveu a seguinte estrutura:

```text
ALUNO(
    id_aluno PK,
    nome_aluno,
    disciplina_1,
    disciplina_2,
    disciplina_3
)
```

**Exemplo:**

| id_aluno | nome_aluno | disciplina_1 | disciplina_2 | disciplina_3 |
|---|---|---|---|---|
| 1 | Ana | Banco de Dados | Algoritmos | Engenharia de Software |
| 2 | Bruno | Banco de Dados | Redes | NULL |
| 3 | Maria | Algoritmos | NULL | NULL |

**Atividades:**

a) Identifique o problema dessa estrutura.

b) Explique por que a existência das colunas `disciplina_1`, `disciplina_2` e `disciplina_3` dificulta a manutenção do banco.

c) O que aconteceria se um aluno precisasse cursar cinco disciplinas?

d) Proponha um modelo lógico que represente adequadamente alunos e disciplinas.

e) Considere que uma disciplina pode possuir vários alunos e que um aluno pode cursar várias disciplinas. Identifique a cardinalidade.

f) Implemente a solução em SQL.

g) Cadastre três alunos e quatro disciplinas.

h) Registre as matrículas dos alunos.

i) Escreva uma consulta que apresente todas as matrículas do aluno de identificador 1.

---

# Parte 4 — Segunda Forma Normal (2FN)

## Exercício 7 — Dependência parcial

Uma loja utiliza a seguinte relação:

```text
ITEM_PEDIDO(
    id_pedido,
    id_produto,
    nome_produto,
    quantidade
)
```

A chave primária é composta:

```text
(id_pedido, id_produto)
```

Considere as regras:

- Um pedido pode conter vários produtos.
- Um produto pode aparecer em vários pedidos.
- Cada produto possui um nome.
- A quantidade depende do produto dentro de determinado pedido.

**Dados:**

| id_pedido | id_produto | nome_produto | quantidade |
|---|---|---|---|
| 1 | 10 | Mouse | 2 |
| 1 | 20 | Teclado | 1 |
| 2 | 10 | Mouse | 3 |
| 3 | 10 | Mouse | 1 |

**Atividades:**

a) Identifique a chave primária composta.

b) Escreva a dependência funcional que determina `quantidade`.

c) Escreva a dependência funcional que determina `nome_produto`.

d) Explique por que `nome_produto` depende apenas de parte da chave primária.

e) A relação está na 1FN? Justifique.

f) A relação está na 2FN? Justifique.

g) Normalize a relação até a 2FN.

h) Apresente o modelo lógico resultante, incluindo uma relação para representar os pedidos.

### Parte prática — SQL

i) Implemente as tabelas normalizadas.

j) Defina a chave primária composta na tabela de itens.

k) Defina as chaves estrangeiras.

l) Cadastre três pedidos.

m) Cadastre dois produtos.

n) Cadastre os itens apresentados na tabela original.

o) Tente cadastrar o mesmo produto duas vezes no mesmo pedido, utilizando a mesma combinação de identificadores.

p) Explique qual restrição impediu a operação.

---

## Exercício 8 — Analisando a 2FN

Considere a relação:

```text
VENDA(
    id_venda PK,
    data_venda,
    id_cliente,
    nome_cliente
)
```

**Regras:**

- Cada venda possui um identificador único.
- Cada venda pertence a um cliente.
- Um cliente pode realizar várias vendas.
- O identificador do cliente determina seu nome.

**Atividades:**

a) Identifique a chave primária.

b) A chave primária é simples ou composta?

c) A relação apresenta dependências parciais de uma chave candidata? Justifique.

d) Considerando que a relação está na 1FN e que sua única chave candidata é `id_venda`, ela atende à 2FN?

e) A relação pode apresentar problemas mesmo estando na 2FN?

f) Identifique a dependência funcional responsável pelo problema.

g) Indique qual forma normal deve ser analisada em seguida.

**Atenção:** nem toda relação com informações repetidas viola a 2FN.

---

# Parte 5 — Terceira Forma Normal (3FN)

## Exercício 9 — Dependência transitiva

Considere novamente:

```text
ALUNO_CURSO(
    id_aluno PK,
    nome_aluno,
    id_curso,
    nome_curso,
    coordenador
)
```

**Regras:**

- Cada aluno pertence a um único curso.
- Cada curso pode possuir vários alunos.
- Cada curso possui um nome e um coordenador.
- A única chave candidata da relação é `id_aluno`.

**Atividades:**

a) Identifique a chave primária.

b) Escreva todas as dependências funcionais relevantes.

c) Identifique as dependências transitivas em relação à chave primária.

d) Explique por que a relação está na 2FN.

e) Explique por que a relação não está na 3FN.

f) Normalize a relação até a 3FN.

g) Apresente o modelo lógico normalizado.

h) Identifique as chaves primárias e estrangeiras.

### Parte prática — SQL

i) Implemente as tabelas normalizadas.

j) Cadastre os seguintes cursos:

| id_curso | nome_curso | coordenador |
|---|---|---|
| 10 | Sistemas de Informação | Carlos |
| 20 | Ciência da Computação | Fernanda |
| 30 | Engenharia de Software | Roberto |

k) Cadastre cinco alunos, distribuídos entre os cursos.

l) Altere o coordenador do curso de Sistemas de Informação para `Marcos`.

m) Consulte o curso alterado.

n) Explique por que não foi necessário atualizar os registros dos alunos.

---

## Exercício 10 — Normalização de funcionários e departamentos

Considere:

```text
FUNCIONARIO_DEPARTAMENTO(
    id_funcionario PK,
    nome_funcionario,
    salario,
    id_departamento,
    nome_departamento,
    gerente_departamento
)
```

**Dados:**

| id_funcionario | nome_funcionario | salario | id_departamento | nome_departamento | gerente_departamento |
|---|---|---:|---|---|---|
| 1 | Ana | 3500 | 10 | TI | Carlos |
| 2 | Bruno | 4200 | 10 | TI | Carlos |
| 3 | Maria | 3100 | 20 | Financeiro | Fernanda |
| 4 | Pedro | 3900 | 10 | TI | Carlos |
| 5 | Julia | 3700 | 20 | Financeiro | Fernanda |

**Atividades:**

a) Identifique as dependências funcionais.

b) Verifique se a relação atende à 1FN.

c) Verifique se a relação atende à 2FN.

d) Verifique se a relação atende à 3FN.

e) Identifique possíveis anomalias de manipulação.

f) Normalize a estrutura até a 3FN.

g) Apresente o modelo lógico resultante.

### Parte prática — SQL

h) Implemente as tabelas normalizadas.

i) Cadastre os departamentos.

j) Cadastre os funcionários.

k) Aumente em 10% o salário dos funcionários do departamento de TI.

l) Altere o gerente do departamento Financeiro.

m) Consulte os dados dos departamentos.

n) Consulte os funcionários que pertencem ao departamento de TI.

o) Tente cadastrar um funcionário associado a um departamento inexistente.

p) Explique o resultado obtido.

---

# Parte 6 — Normalização e SQL

## Exercício 11 — Comparando uma estrutura não normalizada com uma normalizada

Neste exercício, você deverá implementar duas versões de um banco de dados.

### Cenário

Uma loja deseja armazenar produtos e suas categorias.

**Versão A — Estrutura não normalizada até a 3FN**

```text
PRODUTO_CATEGORIA(
    id_produto PK,
    nome_produto,
    preco,
    id_categoria,
    nome_categoria
)
```

**Versão B — Estrutura normalizada**

```text
CATEGORIA(
    id_categoria PK,
    nome_categoria
)

PRODUTO(
    id_produto PK,
    nome_produto,
    preco,
    id_categoria FK
)
```

### Atividades

a) Implemente a versão A utilizando SQL.

b) Cadastre cinco produtos distribuídos em duas categorias.

c) Execute uma consulta para apresentar todos os registros.

d) Altere o nome de uma categoria em apenas um dos produtos.

e) Consulte novamente os registros.

f) Identifique a inconsistência gerada.

g) Implemente a versão B.

h) Cadastre as categorias.

i) Cadastre os mesmos produtos.

j) Altere o nome de uma categoria.

k) Consulte os registros da tabela `CATEGORIA`.

l) Explique por que a versão B reduz o risco de inconsistências.

m) Compare a quantidade de registros que precisam ser alterados nas duas versões.

**Questão para discussão:**

> A normalização elimina todos os problemas de integridade de dados ou ainda precisamos utilizar restrições SQL?

Justifique.

---

## Exercício 12 — Migrando dados de uma tabela não normalizada

Considere a seguinte tabela:

```sql
CREATE TABLE produto_original (
    id_produto BIGINT PRIMARY KEY,
    nome_produto VARCHAR(100) NOT NULL,
    preco NUMERIC(10,2) NOT NULL,
    id_categoria BIGINT NOT NULL,
    nome_categoria VARCHAR(100) NOT NULL
);
```

Insira os registros:

```sql
INSERT INTO produto_original
(id_produto, nome_produto, preco, id_categoria, nome_categoria)
VALUES
(1, 'Notebook', 3500.00, 10, 'Informática'),
(2, 'Mouse', 80.00, 10, 'Informática'),
(3, 'Teclado', 150.00, 10, 'Informática'),
(4, 'Monitor', 1200.00, 20, 'Eletrônicos'),
(5, 'Webcam', 180.00, 20, 'Eletrônicos'),
(6, 'Impressora', 900.00, 30, 'Escritório');
```

### Atividades

a) Identifique os problemas de normalização da tabela.

b) Identifique as dependências funcionais.

c) Proponha o modelo lógico normalizado até a 3FN.

d) Crie as tabelas `categoria_normalizada` e `produto_normalizado`.

e) Utilize `INSERT INTO ... SELECT` para transferir as categorias da tabela original para a tabela normalizada.

**Dica:** investigue o funcionamento de `DISTINCT` para evitar inserir categorias repetidas.

f) Utilize `INSERT INTO ... SELECT` para transferir os produtos.

g) Consulte os dados das novas tabelas.

h) Verifique se todos os produtos foram preservados.

i) Explique por que os identificadores originais das categorias devem ser preservados durante a migração.

j) Explique como a chave estrangeira contribui para a integridade dos dados após a normalização.

**Importante:** não exclua a tabela original durante a atividade.

---

# Parte 7 — Desafio final de normalização

## Exercício 13 — Sistema de gerenciamento de pedidos

Uma empresa desenvolveu a seguinte relação para armazenar seus pedidos:

```text
PEDIDO_PRODUTO(
    id_pedido,
    id_produto,
    data_pedido,
    id_cliente,
    nome_cliente,
    telefone_cliente,
    nome_produto,
    preco_atual_produto,
    quantidade,
    preco_unitario_venda
)
```

A chave primária é composta:

```text
(id_pedido, id_produto)
```

### Regras de negócio

1. Cada pedido possui um identificador único.
2. Cada pedido pertence a um único cliente.
3. Um cliente pode realizar vários pedidos.
4. Cada cliente possui um identificador, nome e telefone.
5. Cada produto possui um identificador, nome e preço cadastral atual.
6. Um pedido pode conter vários produtos.
7. Um produto pode aparecer em vários pedidos.
8. A quantidade depende do produto dentro de determinado pedido.
9. O preço unitário de venda representa o valor cobrado pelo produto naquele pedido, podendo ser diferente do preço cadastral atual.
10. Cada produto aparece no máximo uma vez em cada pedido.

### Etapa 1 — Análise

a) Identifique todos os atributos.

b) Identifique a chave primária.

c) Identifique as dependências funcionais.

d) Identifique as dependências parciais.

e) Identifique as dependências transitivas.

f) Explique por que a relação não está na 2FN.

g) Explique quais problemas impedem que a estrutura atenda à 3FN.

### Etapa 2 — Normalização

h) Normalize a relação até a 1FN.

i) Normalize a relação até a 2FN.

j) Normalize a relação até a 3FN.

k) Apresente o modelo lógico final.

l) Identifique as chaves primárias.

m) Identifique as chaves estrangeiras.

n) Determine as cardinalidades dos relacionamentos.

### Etapa 3 — Implementação SQL

o) Crie as tabelas normalizadas no Supabase.

p) Defina as restrições de integridade necessárias.

q) Cadastre três clientes.

r) Cadastre cinco produtos.

s) Cadastre quatro pedidos.

t) Cadastre pelo menos oito itens de pedido, respeitando a chave primária composta.

u) Consulte todos os pedidos.

v) Consulte os itens pertencentes a um pedido específico.

w) Atualize o telefone de um cliente.

x) Atualize o preço cadastral de um produto.

y) Verifique se a alteração do preço cadastral preservou o preço unitário registrado nos pedidos anteriores.

z) Explique por que essa separação é importante para manter o histórico das vendas.

---