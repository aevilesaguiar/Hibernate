# HQL

## O que é HQL?

HQL é uma abreviação de hibernate query language. Hibernate é uma plataforma para conectar os bancos de dados tradicionais à linguagem orientada a objetos 
(especificamente JAVA). É uma linguagem de consulta em hibernação semelhante ao SQL no RDBMS tradicional exceto pelo fato de utilizarmos uma entidade em HQL
ao invés de tabelas. Ele é escrito embutido em código JAVA e várias funções da biblioteca JAVA são usadas para converter HQL em SQL. Ele pode ser chamado 
como uma linguagem orientada a objetos gravada com instruções de consulta SQL. É uma linguagem flexível e amigável, com sintaxe e gramática próprias para 
recuperar, armazenar e atualizar informações do banco de dados. Reduz a incompatibilidade de impedância entre JAVA e RDBMS.

O que é um Banco de Dados Relacional ( RDBMS )? Um banco de dados relacionado é um tipo de banco de dados que armazena e fornece acesso a pontos de dados 
relacionados entre si. Dados de relacionamento são intuitivos e não modelo relacional, uma maneira direta de representar dados em tabelas.


## Por que precisamos de HQL?

À medida que a importância do JAVA como linguagem para plataformas como a Internet está aumentando, achamos mais relevante conectar nosso aplicativo baseado 
em JAVA ao back-end com a ajuda do hibernate. O Hibernate usa a linguagem HQL para estabelecer a conexão entre o banco de dados e o front-end.

Precisamos de HQL quando queremos selecionar alguns campos e colunas específicos de acordo com nossos requisitos. Os métodos adotados anteriormente 
não foram eficientes o suficiente para detalhar esse nível, por exemplo, buscar o conjunto de resultados ou conjunto de dados do banco de dados como 
um registro inteiro com o número de linhas e colunas. Essa abordagem não oferece flexibilidade para restringir a pesquisa e torna o aplicativo pesado e lento. 
Essa abordagem é usada por conectores JDBC, asp.net e muitos outros idiomas. O uso de HQL reduz esse intervalo de tempo e fornece resultados específicos. 
Portanto, é mais relevante ser usado em um ambiente de tempo real onde o JAVA esteja envolvido no front-end.

## Como funciona o HQL?

HQL é um formato de arquivo XML para vincular java do front-end ao banco de dados no back-end. As consultas SQL que disparamos no banco de dados
diretamente usando consultas sql também podem ser escritas em hql. O HQL possui uma sintaxe própria onde podemos escrever a consulta e então 
essa consulta é convertida em instruções SQL que podem ser compreendidas pelo banco de dados. Isso é escrito em linguagem Java para reduzir 
a incompatibilidade de impedância.

HQL é uma linguagem que não diferencia maiúsculas de minúsculas, exceto para o nome de classes e entidades. Por exemplo: org.hibernate.eg.test não é 
igual a org.hibernate.eg.Test já que “test” e “Test” são duas entidades diferentes em HQL.

Nota: Podemos usar SQL em consultas HQL diretamente usando o código nativo.

## Vantagens do HQL

Existem várias vantagens do HQL como linguagem:

O codificador não tem nenhuma obrigação de aprender a linguagem SQL.
O HQL é orientado a objetos e seu desempenho é bom quando vinculamos nosso aplicativo front-end ao back-end.
HQL tem memória cache e, portanto, melhora a velocidade.
HQL suporta recursos populares de conceitos de POO como polimorfismo, herança e associação.


## Sintaxe junto com exemplos de consulta HQL

Algumas consultas simples em hibernate se parecem com:

Usando a cláusula FROM:

From eg.Test or From Test.

Esta instrução retornará todas as instâncias da classe. Neste caso, é Teste. Também podemos criar um alias para, 
por exemplo: From Test como um teste. Aqui, “teste” é o alias de Test. Esse alias pode ser usado posteriormente em vez de classe.

Exemplo 1

Código:

String hqlquery = "FROM Test";
Query q = session.createQuery(hqlquery);
List display = q.list();
AS Clause: From eg.Test AS T or From Test  AS T.


Esta declaração é usada quando queremos criar aliases para as classes principais de HQL. Esta é uma técnica útil caso tenhamos consultas longas. 
Podemos simplesmente atribuir a consulta ao alias e usar esse alias para tratamento de dados adicional. O aliasing também pode ser feito 
sem a palavra-chave AS. Por exemplo: Do teste T.

- Exemplo #2

String hqlquery = "FROM Test AS T";
Query q = session.createQuery(hqlquery);
List display = q.list();
WHERE Clause: From eg.Test T WHERE T.code=102 or From Test T WHERE T.code=102.


Esta cláusula é usada quando estamos pesquisando um dado específico na tabela do banco de dados. Portanto, aqui, se estivermos procurando 
por um registro específico com base no código de teste que temos, essa cláusula será usada na consulta. Isso ajudará a restringir o critério de 
pesquisa. Se fornecermos a chave primária da tabela na cláusula where, veremos uma melhoria significativa na velocidade de busca.


- Exemplo #3

String hqlquery = "FROM Test T WHERE T.code = 102";
Query q = session.createQuery(hqlquery);
List display = q.list();
SELECT Clause:
From eg. SELECT T.number FROM Test T.

Esta cláusula é usada se quisermos selecionar uma coluna específica da tabela do banco de dados. Essa é uma das maneiras de restringir o critério de busca. 
Qualquer que seja o nome do campo que dermos na cláusula select, somente ele será selecionado. É útil buscar uma pequena quantidade de dados se tivermos 
informações específicas sobre os mesmos.


- Exemplo #4

String hql = "SELECT E.firstName FROM Employee E";
Query query = session.createQuery(hql);
List results = query.list();
DELETE Clause:
String hqlexample = "DELETE FROM Test "  +
"WHERE code = : test_code";

Esta cláusula na consulta pode ser usada para excluir um ou mais objetos da tabela de banco de dados conectada. Ambos os objetos “transitórios” e “persistentes” 
podem ser excluídos desta forma. Esta é a consulta simples para excluir qualquer número de campos ou tabelas do banco de dados. Isso deve ser usado com cuidado.



- Exemplo #5

String hqlquery =  "DELETE FROM Test "  +
"WHERE code = : test_code";
Query q = session.createQuery(hqlquery);
q.setParameter("test_code", 102);
int display = q.executeUpdate();
System.out.println("Hence the number of rows modified are: " + display);

## Conclusão

Portanto, HQL é uma elegante linguagem orientada a objetos que preenche a lacuna entre o JAVA orientado a objetos e o sistema de gerenciamento de banco de dados. 
Com a maior participação de mercado, a linguagem de consulta de hibernação está se tornando uma linguagem popular para trabalhar.



## to-do


https://www.educba.com/hibernate-framework/
https://www.educba.com/hibernate-session/
https://www.educba.com/hibernate-interview-questions/
https://www.educba.com/what-is-java-hibernate/

## Referencias

- https://www.educba.com/hql/

