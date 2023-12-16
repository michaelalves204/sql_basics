## Explorando Comandos Básicos de SQL com PostgreSQL

Nesta publicação, vamos abordar os comandos fundamentais de SQL utilizando o PostgreSQL. O objetivo é proporcionar uma visão prática, abrangendo a criação de um banco de dados, a definição de tabelas, a inserção de registros e a execução de consultas. 


### Utilizando o PostgreSQL no Terminal do Ubuntu

Atualmente, estou utilizando o PostgreSQL por meio do terminal no sistema operacional Ubuntu. Abaixo estão os links para a instalação do banco de dados:

[Ubuntu](https://www.postgresql.org/download/linux/ubuntu/)

[Mac](https://www.postgresql.org/download/macosx/)

Configurei o PostgreSQL para um usuário denominado 'postgres' com a senha 'postgres'. Para acessar o banco, utilizo o seguinte comando no terminal:

```bash

psql -U postgres -h localhost -W

```

Nesse comando, '-U' especifica o usuário, '-h' indica o host (localhost no meu caso), e '-W' solicita a senha, onde informo a senha configurada anteriormente.

Iniciaremos nossa jornada no SQL com o comando CREATE DATABASE. Este comando é utilizado para criar o nosso banco de dados. Vejamos como aplicá-lo:

```sql
CREATE DATABASE test;
```

Com esse simples comando, estabelecemos a base para nosso banco de dados denominado 'test'. Após a criação do banco de dados, surge a necessidade de visualizar todos os bancos disponíveis. No PostgreSQL, podemos fazer isso utilizando o comando \list:

```sql
\list
```

Este comando proporciona uma lista completa de todos os bancos de dados presentes, permitindo uma visão geral de nossos recursos.

### Acessando o Novo Banco de Dados Criado

Para acessar o banco de dados recentemente criado, utilizamos o comando:

```sql
\c test
```

Este comando simples, "\c test", permite estabelecer uma conexão direta com o banco de dados denominado 'test'. Ao executar essa instrução, entramos no ambiente do banco de dados, onde podemos realizar operações, consultas e atualizações.

### Criando a Primeira Tabela no Banco de Dados Test

Após acessar o banco de dados 'test', é hora de dar o próximo passo e criar nossa primeira tabela. 

```sql
CREATE TABLE users (id SERIAL PRIMARY KEY, name VARCHAR NOT NULL, email VARCHAR UNIQUE NOT NULL, created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP, updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP);
```

(É possivel usar o comando IF NOT EXISTS apos CREATE TABLE, esse comando serve como uma validação ou seja para tentar criar a tabela apenas se ela não existir no banco de dados)

Neste comando, estamos definindo uma tabela chamada 'users', com colunas como 'id', 'name', e 'email', entre outras. Além disso, especificamos restrições, como a obrigatoriedade de um nome ('name' NOT NULL) e a unicidade de e-mails ('email' UNIQUE NOT NULL). As colunas 'created_at' e 'updated_at' são configuradas para registrar automaticamente as timestamps de criação e atualização.

Para confirmar se a tabela foi criada com sucesso, utilizamos o comando:

```sql
\dt
```
Exemplo de retorno

```bash
List of relations
 Schema | Name  | Type  |  Owner
--------+-------+-------+----------
 public | users | table | postgres
```

### Inserindo um Novo Registro na Tabela 'users'

Vamos avançar criando um novo registro na nossa tabela 'users'.

```sql
INSERT INTO users (name, email) VALUES ('michael', 'michael@gmail.com');
```
Neste comando, estamos adicionando um novo usuário à tabela 'users' com um nome específico ('michael') e um endereço de e-mail ('michael@gmail.com'). Ao executar esta instrução, o PostgreSQL registrará automaticamente as timestamps de criação e atualização, conforme definido anteriormente na estrutura da tabela.

Podemos criar varias usuarios (lembrando que o endereço de email não pode se repetir devido a validação que colocamos onde o email deve ser unico):

Criando mais um usuario

```sql
INSERT INTO users (name, email) VALUES ('pedro', 'pedro@gmail.com');
```

### Explorando Comandos de Consulta no PostgreSQL

Vamos agora explorar diferentes comandos de consulta para obter informações da tabela 'users':

#### Listar Todos os Usuários:

```sql
SELECT * FROM users;
```

Este comando retorna todos os registros da tabela 'users', exibindo todas as colunas disponíveis.

#### Listar um Campo Específico (no caso, 'name'):

```sql
SELECT name FROM users;
```

Utilizando essa instrução, obtemos uma lista contendo apenas os nomes dos usuários, focando em um campo específico da tabela.

#### Listar Através de um Parâmetro (por exemplo, nome 'Michael'):

```sql
SELECT * FROM users WHERE name = 'michael';
```

Com este comando, filtramos os resultados para exibir apenas os registros onde o campo 'name' é igual a 'michael'.

### Ampliando as Possibilidades: Filtros e Busca Sensível e Insensível a Maiúsculas/Minúsculas

Além das consultas básicas, temos a capacidade de aprimorar nossas buscas utilizando filtros. Vamos explorar duas técnicas:

#### Filtrar por Parte do Nome:

```sql
SELECT * FROM users WHERE name LIKE '%m%';
```

Este comando retorna registros onde o campo 'name' contém a letra 'm', independentemente da posição na string. O símbolo '%' funciona como um curinga.

#### Filtrar Insensível a Maiúsculas/Minúsculas:

```sql
SELECT * FROM users WHERE name ILIKE '%M%';
```
Utilizando o operador ILIKE, obtemos resultados de maneira insensível a maiúsculas e minúsculas. Assim, a busca por '%M%' retornará 'michael', 'maria', entre outros.

Esses filtros adicionam uma camada de flexibilidade às nossas consultas, permitindo-nos refinar as buscas de acordo com critérios específicos.(É fundamental lembrar que, embora os comandos apresentados sejam básicos e eficazes para aprendizado, certos tipos de filtros, como os demonstrados, podem impactar negativamente o desempenho, especialmente em projetos de produção com grande escala de uso.)

### Atualização e Exclusão de Registros

Vamos explorar agora comandos essenciais para a manipulação de dados no PostgreSQL: a atualização e a exclusão de registros.

#### Atualização de Registros:

```sql
UPDATE users SET name = 'Michael Alves' WHERE name = 'michael';
```

Com este comando, realizamos a atualização do campo 'name' na tabela 'users'. No exemplo, alteramos o nome de 'michael' para 'Michael Alves'. Certifique-se de fornecer a condição adequada após o WHERE para direcionar a atualização corretamente.

#### Exclusão de Registros:

```sql
DELETE FROM users WHERE id = 1;
```

Este comando exclui registros da tabela 'users' com base em uma condição específica. No exemplo, excluímos o registro com o ID igual a 1. Utilize com cautela, garantindo que a condição seja precisa para evitar exclusões indesejadas.


### Estabelecendo Relacionamento entre Tabelas: Chave Estrangeira no PostgreSQL

Agora, avançaremos na criação de uma nova tabela que estabelecerá uma conexão com a tabela 'users'. Essa relação será fundamentada em uma chave estrangeira chamada 'user_id'.

#### Criando a Tabela 'posts':

```sql
CREATE TABLE posts (
    id SERIAL PRIMARY KEY,
    title VARCHAR NOT NULL,
    user_id INTEGER REFERENCES users(id)
);
```

Neste comando, definimos a tabela 'posts' com colunas para 'id', 'title', e 'user_id', que funciona como uma chave estrangeira referenciando a coluna 'id' da tabela 'users'.

#### Povoando a Tabela 'posts':

```sql
INSERT INTO posts (title, user_id) VALUES ('test', 1);
INSERT INTO posts (title, user_id) VALUES ('test2', 1);
```
Aqui, inserimos dois registros iniciais na tabela 'posts', associando-os a usuários específicos através da chave estrangeira 'user_id'.

Criando Novos Posts:

```sql
INSERT INTO posts (title, user_id) VALUES ('test3', 1);
```
Esses comandos adicionam novos posts à tabela, continuando a estabelecer vínculos com usuários por meio da chave estrangeira.

Este é um exemplo básico de como criar e usar chaves estrangeiras para relacionar tabelas em um banco de dados PostgreSQL.

Vamos listar todos os registros em posts
```sql
SELECT * FROM posts;
```
Exemplo de retorno:

````sql
test=# SELECT * FROM posts;
 id | title | user_id
----+-------+---------
  1 | test  |       1
  2 | test2 |       1
  3 | test3 |       1
````

### Explorando Diferentes Tipos de JOIN no PostgreSQL

Vamos aprofundar nossa compreensão dos JOINs no PostgreSQL, utilizando diferentes abordagens para recuperar informações de tabelas relacionadas.

#### INNER JOIN:

```sql
SELECT posts.* FROM posts INNER JOIN users ON posts.user_id = users.id WHERE users.id = 3;
```

Neste exemplo, utilizamos um INNER JOIN para obter registros que possuem correspondências nas tabelas 'posts' e 'users', onde o 'user_id' na tabela 'posts' é igual ao 'id' na tabela 'users'. Esta consulta retorna apenas os registros que têm correspondências em ambas as tabelas.

#### LEFT JOIN:

```sql
SELECT posts.* FROM posts LEFT JOIN users ON posts.user_id = users.id WHERE users.id = 3;
```

Ao empregar um LEFT JOIN, obtemos todos os registros da tabela da esquerda ('posts') e os registros correspondentes da tabela da direita ('users'). Mesmo se não houver correspondência na tabela da direita, os registros da tabela da esquerda serão exibidos.

#### RIGHT JOIN:

```sql
SELECT posts.* FROM posts RIGHT JOIN users ON posts.user_id = users.id WHERE users.id = 3;
```

Semelhante ao LEFT JOIN, um RIGHT JOIN retorna todos os registros da tabela da direita ('users') e os registros correspondentes da tabela da esquerda ('posts'). Caso não haja correspondência na tabela da esquerda, os registros da tabela da direita ainda serão apresentados.

#### FULL OUTER JOIN:

```sql
SELECT posts.* FROM posts FULL OUTER JOIN users ON posts.user_id = users.id WHERE users.id = 3;
```
Com um FULL OUTER JOIN, obtemos registros quando há correspondência em uma das tabelas, exibindo tanto os registros correspondentes como os não correspondentes.

Estas são ferramentas poderosas para combinar dados de tabelas relacionadas de maneiras específicas.

### Explorando LIMIT, OFFSET e Contagem de Registros no PostgreSQL

Vamos discutir o uso de LIMIT e OFFSET, que são fundamentais para controlar a quantidade de registros retornados em consultas SQL.

#### Utilizando LIMIT para Restringir Resultados:

```sql
-- Limitando a 2 registros em ordem ascendente (asc)
SELECT * FROM posts ORDER BY id ASC LIMIT 2;
-- Limitando a 2 registros em ordem descendente (desc)
SELECT * FROM posts ORDER BY id DESC LIMIT 2;
```
No primeiro exemplo, limitamos a consulta aos dois primeiros registros da tabela 'posts' em ordem ascendente, e no segundo exemplo, em ordem descendente.

#### Adicionando Paginação com LIMIT e OFFSET:

```sql
-- Página 1: Exibindo 2 registros a partir do início
SELECT * FROM posts ORDER BY id ASC LIMIT 2 OFFSET 0;
-- Página 2: Exibindo 2 registros a partir do terceiro registro
SELECT * FROM posts ORDER BY id ASC LIMIT 2 OFFSET 2;
```
Ao adicionar OFFSET, podemos criar páginas de resultados. O primeiro exemplo exibe os dois primeiros registros (página 1), e o segundo exibe dois registros começando do terceiro (página 2).

#### Contagem de Registros com COUNT:

```sql
-- Contagem total de registros na tabela 'posts'
SELECT COUNT(*) FROM posts;
```

Este exemplo utiliza COUNT(*) para determinar o número total de registros na tabela 'posts'.

### Comandos Poderosos e perigosos

Vamos falar sobre comandos que podem ser considerados "poderosos" - ou talvez até um pouco perigosos.

#### Deletar Tabelas:

```sql
DROP TABLE posts;
DROP TABLE users;
```

Lembre-se, a ordem importa! Se houver dependências entre tabelas, como o vínculo entre 'posts' e 'users', certifique-se de deletar a tabela dependente primeiro. Caso contrário, podem ocorrer mensagens de erro ou resultados inesperados.

#### Deletar Banco de Dados:

```sql
DROP DATABASE test;
```

Para apagar todo um banco de dados, utiliza-se o comando DROP DATABASE. Certifique-se de estar absolutamente certo de que deseja excluir o banco de dados, pois essa ação é irreversível e removerá todos os dados associados.

Lembre-se sempre de praticar o bom senso e, quando possível, realize essas operações em ambientes controlados.


Referências:

https://www.postgresql.org/docs/current/tutorial-start.html

https://www.postgresql.org/docs/current/sql-syntax.html

https://www.postgresql.org/docs/current/datatype.html

