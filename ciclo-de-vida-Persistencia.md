# O Ciclo de Vida da Persistência

## Trabalhando com objetos de entidade JPA

Objetos de entidade são instâncias na memória de classes de entidade (classes persistentes definidas pelo usuário), que podem representar objetos físicos no banco de dados.

Gerenciar um Banco de Dados de Objetos ObjectDB usando JPA requer o uso de objetos de entidade para muitas operações, incluindo armazenamento, recuperação, atualização e exclusão de objetos de banco de dados.

Esta página abrange os seguintes tópicos:

## Ciclo de vida do objeto da entidade

O ciclo de vida dos objetos de entidade consiste em quatro estados: Novo(new), Gerenciado(Managed), Removido(removed)  e Desanexado(detach).

![image](https://user-images.githubusercontent.com/52088444/183268793-ba407aa1-187c-4232-b002-489bb4ff3d56.png)

Quando um objeto de entidade é criado inicialmente, seu estado é New . Nesse estado o objeto ainda não está associado a um EntityManager e não tem representação no banco de dados.


Um objeto de entidade se torna Gerenciado(Managed) quando é persistido no banco de dados por meio de um EntityManager e métodos, que deve ser invocado em uma transação ativa. Na confirmação(commit) da transação, o proprietário EntityManager armazena o novo objeto de entidade no banco de dados. 

Objetos de entidade recuperados do banco de dados por um EntityManager também estão no estado Gerenciado(Managed) . 


Se um objeto de entidade gerenciada for modificado em uma transação ativa, a alteração será detectada pelo proprietário EntityManager e a atualização será propagada para o banco de dados na confirmação da transação.

Um objeto de entidade gerenciada também pode ser recuperado do banco de dados e marcado para exclusão, usando o método dentro de uma transação ativa. O objeto de entidade altera seu estado de Managed para Removed e é excluído fisicamente do banco de dados durante a confirmação.


O último estado, Detached , representa objetos de entidade que foram desconectados do EntityManager. Por exemplo, todos os objetos gerenciados de um EntityManager são desanexados quando o EntityManageré fechado. 

## O contexto de persistência

O contexto de persistência é a coleção de todos os objetos gerenciados de um arquivo EntityManager. Se um objeto de entidade que deve ser recuperado já existir no contexto de persistência, o objeto de entidade gerenciado existente será retornado sem realmente acessar o banco de dados (exceto recuperação por refresh, que sempre requer acesso ao banco de dados).

A principal função do contexto de persistência é garantir que um objeto de entidade de banco de dados seja representado por não mais do que um objeto de entidade na memória dentro do mesmo arquivo EntityManager. Cada EntityManager gerencia seu próprio contexto de persistência. Portanto, um objeto de banco de dados pode ser representado por diferentes objetos de entidade de memória em diferentes EntityManager instâncias. Mas recuperar o mesmo objeto de banco de dados mais de uma vez usando o mesmo EntityManager deve sempre resultar no mesmo objeto de entidade na memória.

Outra maneira de ver isso é que o contexto de persistência também funciona como um cache local para um determinado arquivo EntityManager. O ObjectDB também gerencia um cache compartilhado de nível 2  para oEntityManagerFactory  cache , bem como outros caches, conforme explicado no capítulo Configuração .

Por padrão, objetos de entidade gerenciada que não foram modificados ou removidos durante uma transação são mantidos no contexto de persistência por referências fracas. Portanto, quando um objeto de entidade gerenciada não está mais em uso pelo aplicativo, o coletor de lixo pode descartá-lo e ele é automaticamente removido do contexto de persistência. O ObjectDB pode ser configurado para usar referências fortes ou referências flexíveis em vez de referências fracas .

O containsmétodo pode verificar se um objeto de entidade especificado está no contexto de persistência:

  boolean isManaged = em.contains(employee);
  
O contexto de persistência pode ser limpo usando o método:clear

    em.clear();
    
Quando o contexto de persistência é limpo, todas as suas entidades gerenciadas são desanexadas e quaisquer alterações nos objetos de entidade que não foram liberadas para o banco de dados são descartadas.



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



