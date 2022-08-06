# O Ciclo de Vida da Persistência

## Trabalhando com objetos de entidade JPA

Objetos de entidade são instâncias na memória de classes de entidade (classes persistentes definidas pelo usuário), que podem representar objetos físicos no banco de dados.

Gerenciar um Banco de Dados de Objetos ObjectDB usando JPA requer o uso de objetos de entidade para muitas operações, incluindo armazenamento, recuperação, atualização e exclusão de objetos de banco de dados.

Esta página abrange os seguintes tópicos:

## Ciclo de vida do objeto da entidade

O ciclo de vida dos objetos de entidade consiste em quatro estados: Novo(new), Gerenciado(Managed), Removido(removed)  e Desanexado(detach).

![image](https://user-images.githubusercontent.com/52088444/183268793-ba407aa1-187c-4232-b002-489bb4ff3d56.png)

Quando um objeto de entidade é criado inicialmente, seu estado é New . Nesse estado o objeto ainda não está associado a um e não tem representação no banco de dados.EntityManager





## javax.persistence.EntityManager - interface JPA

javax.persistence
Interface EntityManager

Interface usada para interagir com o contexto de persistência.

Uma EntityManagerinstância está associada a um contexto de persistência. Um contexto de persistência é um conjunto de instâncias de entidade em que para qualquer 
identidade de entidade persistente existe uma instância de entidade única. Dentro do contexto de persistência, as instâncias da entidade e seu ciclo de vida são
gerenciados. A EntityManagerAPI é usada para criar e remover instâncias de entidades persistentes, para localizar entidades por sua chave primária e para consultar
entidades.

O conjunto de entidades que podem ser gerenciadas por uma determinada EntityManagerinstância é definido por uma unidade de persistência. Uma unidade de persistência
define o conjunto de todas as classes que estão relacionadas ou agrupadas pela aplicação, e que devem ser colocadas em seu mapeamento para um único banco de dados.

Um objeto de entidade se torna Gerenciado quando é persistido no banco de dados por meio de um método, que deve ser invocado em uma transação ativa. Na confirmação da transação, 
o proprietário armazena o novo objeto de entidade no banco de dados.

Objetos de entidade recuperados do banco de dados por um EntityManagertambém estão no estado Gerenciado . A recuperação de objetos é discutida com mais detalhes 
na seção Recuperando entidades .






## Armazenando objetos de entidade JPA


Novos objetos de entidade podem ser armazenados no banco de dados explicitamente invocando o método ou implicitamente como resultado de uma operação em cascata


## Referencias

https://www.objectdb.com/java/jpa/persistence/managed
https://www.objectdb.com/api/java/jpa/EntityManager
https://www.objectdb.com/java/jpa/persistence/store
https://www.objectdb.com/java/jpa/persistence/managed



