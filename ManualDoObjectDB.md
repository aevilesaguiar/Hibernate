# Manual do ObjectDB

## Definindo uma classe de entidade JPA

Para poder armazenar objetos no banco de dados usando JPA, precisamos definir uma classe de entidade . Uma classe de entidade JPA é uma classe POJO
(Plain Old Java Object), ou seja, uma classe Java comum que é marcada (anotada) como tendo a capacidade de representar objetos no banco de dados.

## The Point Entity Class

package com.objectdb.tutorial;

import javax.persistence.Entity;

@Entity
public class Point {
    // Persistent Fields:
    private int x;
    private int y;

    // Constructor:
    Point (int x, int y) {
        this.x = x;
        this.y = y;
    }

    // Accessor Methods:
    public int getX() { return this.x; }
    public int getY() { return this.y; }

    // String Representation:
    @Override
    public String toString() {
        return String.format("(%d, %d)", this.x, this.y);
    }
}

Como você pode ver acima, uma classe de entidade é uma classe Java comum. A única adição JPA exclusiva é a anotação, que marca a classe como uma classe de 
entidade.@Entity

## Campos persistentes em classes de entidade


Armazenar um objeto de entidade no banco de dados não armazena métodos e código. Apenas o estado persistente do objeto de entidade, conforme refletido por 
seus campos persistentes,  é armazenado. Por padrão, qualquer campo que não seja declarado como static ou transientseja um campo persistente. Por exemplo, 
os campos persistentes da Pointclasse de entidade descritos acima são xe y(representando a posição do ponto no plano). São os valores desses campos que são 
armazenados no banco de dados quando um objeto de entidade é persistido.

Se você já estiver familiarizado com JPA, deve ter notado que a Pointclasse de entidade não possui um campo de chave primária ( ). Como um banco de dados de 
objetos, o ObjectDB oferece suporte a IDs de objetos implícitos, portanto, uma chave primária definida explicitamente não é necessária. Por outro lado, o 
ObjectDB também suporta  chaves primárias JPA explícitas , incluindo chaves primárias compostas e geração automática de valor sequencial . Este é um recurso 
muito poderoso do ObjectDB que está ausente de outros bancos de dados orientados a objetos.@Id



## Obtendo uma conexão de banco de dados JPA


No JPA uma conexão de banco de dados é representada pela interface EntityManager. Portanto, para manipular um banco de dados ObjectDB, precisamos  uma instância de EntityManager. 
As operações que modificam o conteúdo do banco de dados também requerem uma instância EntityTransaction.

## Obtendo um EntityManagerFactory

A obtenção de uma instância EntityManager consiste em duas etapas. Primeiro, precisamos obter uma instância de EntityManagerFactory que represente o banco de dados relevante e, em seguida, podemos usar 
essa instância de fábrica para obter uma instância EntityManager .


O JPA requer a definição de uma unidade de persistência em um arquivo XML para poder gerar um arquivo EntityManagerFactory. Mas ao usar o ObjectDB, você pode definir uma 
unidade de persistência padrão em um arquivo XML ou simplesmente fornecer o caminho do arquivo do banco de dados ObjectDB:

EntityManagerFactory emf =
     Persistence.createEntityManagerFactory(
          "objectdb:$objectdb/db/points.odb");
          
O createEntityManagerFactory método estático espera um nome de unidade de persistência como argumento, mas ao usar ObjectDB, qualquer caminho de arquivo de banco de dados válido 
(absoluto ou relativo) também é aceito. Qualquer string que comece com o prefixo objectdb: ou termine com .odb.objectdb ou seja considerada pelo ObjectDB como uma URL de 
banco de dados em vez de um nome de unidade de persistência. A variável representa o diretório inicial do ObjectDB (por padrão - o diretório no qual 
o ObjectDB está instalado). Se ainda não existir nenhum arquivo de banco de dados no caminho fornecido, o ObjectDB tentará criar um nome.

O EntityManagerFactorytambém é usado para fechar o banco de dados quando terminarmos de usá-lo:

 emf.close();

## Obtendo um EntityManager

Uma vez que temos um EntityManagerFactory, podemos facilmente obter uma instância EntityManager:

 EntityManager em = emf.createEntityManager();
 
 A EntityManagerinstância representa uma conexão com o banco de dados. Ao usar o JPA, cada operação em um banco de dados é associada a um arquivo EntityManager. 
 Além disso, em um aplicativo multithread, cada thread geralmente tem sua própria EntityManagerinstância e, ao mesmo tempo, compartilha um único arquivo 
 EntityManagerFactory.
 
 Quando a conexão com o banco de dados não for mais necessária, EntityManagerele poderá ser fechado:
 
 em.close();
 
 Fechar um EntityManager não fecha o banco de dados em si (esse é o trabalho da fábrica, conforme explicado anteriormente). Uma vez que o EntityManager objeto é 
 fechado, ele não pode ser reutilizado. No entanto, EntityManagerFactory  a instância proprietária pode preservar os EntityManager's recursos (como um ponteiro 
 de arquivo de banco de dados ou um soquete para um servidor remoto) em um pool de conexões e usá-los para acelerar a EntityManager na construção futura.
 

## Usando uma EntityTransaction

As operações que modificam o conteúdo do banco de dados, como armazenar, atualizar e excluir, devem ser executadas apenas em uma transação ativa.

Dado um EntityManager , em, é muito fácil iniciar uma transação:

 em.getTransaction().begin();
 
 Há um relacionamento um para um entre uma EntityManager instância e EntityTransaction e suas instâncias associadas que o método getTransaction retorna.
 
 Quando uma transação está ativa, você pode invocar EntityManager métodos que modificam o conteúdo do banco de dados, como persist e remove . As atualizações do 
 banco de dados são coletadas e gerenciadas na memória e aplicadas ao banco de dados quando a transação é confirmada:
 
   em.getTransaction().commit();
   
   
   ## Referencias
   
   https://www.objectdb.com/java/jpa/entity
