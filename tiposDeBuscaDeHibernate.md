# Tipos de busca de hibernate

Em qualquer framework ORM , é muito importante entender como ele carrega a entidade, especialmente quando a entidade possui relações e coleções nela.

No Hibernate , chamamos isso de modo de busca ou estratégia de busca .

Sim O Hibernate decide como carregar a entidade baseado no modo de busca .

Exemplo:

Se a entidade User tiver uma entidade Collection of Address nela.

Quando carregamos o usuário, devemos carregar os endereços também? Ou devemos carregar apenas o usuário?

A resposta a esta pergunta no Hibernate é baseada no modo Fetch que especificamos.

Quando carregamos o usuário , ao mesmo tempo, se carregamos o endereço também , então é chamado de carregamento EAGER(EAGER loading).

Se atrasarmos(delay) o carregamento de endereços durante o carregamento de usuário até que exijamos endereços, isso será chamado de carregamento lento (Lazy loading).

Fetch mode (O modo de busca) nos ajuda a personalizar o número de consultas geradas e a quantidade de dados recuperados.

## Diferentes modos de busca(Fetch modes) suportados pelo Hibernate

1) FetchMode JOIN
Eager loading which loads all the collections and relations at the same time.
Carregamento ansioso que carrega todas as coleções e relações ao mesmo tempo.

2) FetchMode SELECT(default)
Lazy loading which loads the collections and relations only when required.
Lazy loading que carrega as coleções e relações somente quando necessário.

3) FetchMode SELECT with Batch Size
Fetch upto “N”collections or entities(“Not number of records”)
Buscar até “ N ” coleções ou entidades (“Número de registros”)

4) FetchMode SUBSELECT
Group the collection of an entity into a Sub-Select query.
Agrupe a coleção de uma entidade em uma consulta Sub-Select.

## Agora vamos entender esses modos de busca em detalhes com o exemplo abaixo

A entidade do cliente tem uma entidade Coleção de endereços

É uma relação de um para muitos @OneToMany (um cliente terá muitos endereços)

![image](https://user-images.githubusercontent.com/52088444/183411946-7d22157a-76ac-47f5-8eed-6203045a5a66.png)

## Código para definir a estratégia de busca

Podemos definir o modo Fetch por meio de mapeamento XML ou mapeamento de anotação

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-mapping PUBLIC
"-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
 
<hibernate-mapping package="com.kb.model">
    <class name="Customer" table="customers">
        <id name="id" type="int" column="Id">
            <generator class="increment" />
        </id>
        <property name="firstName">
            <column name="FirstName" />
        </property>
        <property name="lastName">
            <column name="LastName" />
        </property>
        <property name="city">
            <column name="City" />
        </property>
       
        <list name="addresses"  cascade="all" inverse="true"
            table="address" fetch="select" batch-size="10">
            <key>
                <column name="CUSTOMER_ID" not-null="true" />
            </key>
            <list-index column="idx" />
               <one-to-many class="Address" />
        </list>
    </class>
</hibernate-mapping>


Podemos ver que ao definir a lista de endereços usando a tag <list> , especificamos o atributo fetch .

Também especificamos o tamanho do lote como 10 , que é necessário no modo de busca em lote.


2) Agora vamos ver como podemos conseguir o mesmo através de anotações

@Entity
@Table(name = "customers", catalog = "javainsimpleway")
public class Customer implements Serializable{
...
	@OneToMany(fetch = FetchType.LAZY, mappedBy = "customers")
	@Cascade(CascadeType.ALL)
	@Fetch(FetchMode.SELECT)
        @BatchSize(size = 10)
	public List<Address> getAddresses() {
		return this.addresses;
	}
...
}

Podemos ver que ao definir a relação One to Many usando a anotação @OneToMany , especificamos o atributo fetch .

Também especificamos o tamanho do lote como 10 usando a anotação @BatchSize que é necessária no modo de busca em lote.

Usaremos esses 2 arquivos de mapeamento (XML e Anotação) para abordar cada Modo de busca conforme abaixo

## FetchMode JOIN

Modifique o xml acima com fetch=”join”

(ou)

Modifique a classe anotada acima com @Fetch(FetchMode.JOIN)

Isso também é chamado de Eager loading , o que significa carregar todas as coleções e relações , independentemente de usá-las ou não.

Essa estratégia de busca carrega Usuário e Lista de Endereços em uma única consulta quando solicitamos apenas Usuário.

Ele carregará todos os endereços quando carregarmos o usuário , independentemente de usarmos o endereço ou não.

Customer  customer = session.get(Customer.class, customerId);
List<Address> addresses = customer.getAddresses();

-  Query generated from Hibernate

Hibernate:
 
    select

        customer0_.Id as Id1_1_0_,

        customer0_.FirstName as FirstNam2_1_0_,

        customer0_.LastName as LastName3_1_0_,

        customer0_.City as City4_1_0_,

        addresses1_.CUSTOMER_ID as CUSTOMER5_0_1_,

        addresses1_.Id as Id1_0_1_,

        addresses1_.idx as idx6_1_,

        addresses1_.Id as Id1_0_2_,

        addresses1_.Street as Street2_0_2_,

        addresses1_.Landmark as Landmark3_0_2_,

        addresses1_.PostalCode as PostalCo4_0_2_,

        addresses1_.CUSTOMER_ID as CUSTOMER5_0_2_ 

    from

        customers customer0_ 

    left outer join

        address addresses1_ 
                          on customer0_.Id=addresses1_.CUSTOMER_ID 

    where

        customer0_.Id=?
        
        
        
O Hibernate gerou apenas uma instrução select , ele recupera todas as suas coleções relacionadas quando o Cliente é carregado usando session.get(Customer.class, 1)

Selecione a instrução para recuperar os registros do cliente

Outer join para recuperar suas coleções relacionadas .


## FetchMode SELECT(padrão)

Modifique o xml acima com fetch=”select”

(ou)

Modifique a classe anotada acima com @Fetch(FetchMode.SELECT)

Isso também é chamado de carregamento lento, o que significa que carrega as coleções e relações somente quando necessário

Ele carrega apenas o Cliente quando solicitamos ao Cliente, não carregará os endereços até que solicitemos.

Ele carregará todos os endereços somente quando solicitarmos explicitamente os endereços a serem usados ​​em nosso aplicativo.

Customer  customer = session.get(Customer.class, customerId); //Select from Customer table only

List<Address> addresses = customer.getAddresses();
 
//Select from Address table will start here
	for (Address address : addresses) {
			System.out.println(address);
		}
   
   
- Query generated from Hibernate

Hibernate:

    select

        customer0_.Id as Id1_1_0_,

        customer0_.FirstName as FirstNam2_1_0_,

        customer0_.LastName as LastName3_1_0_,

        customer0_.City as City4_1_0_ 

    from

        customers customer0_ 

    where

        customer0_.Id=?


Hibernate: 

    select

        addresses0_.CUSTOMER_ID as CUSTOMER5_0_0_,

        addresses0_.Id as Id1_0_0_,

        addresses0_.idx as idx6_0_,

        addresses0_.Id as Id1_0_1_,

        addresses0_.Street as Street2_0_1_,

        addresses0_.Landmark as Landmark3_0_1_,

        addresses0_.PostalCode as PostalCo4_0_1_,

        addresses0_.CUSTOMER_ID as CUSTOMER5_0_1_ 

    from

        address addresses0_ 

    where

        addresses0_.CUSTOMER_ID=?
        
        
Hibernate gerou 2 instruções select

Primeira instrução Select para recuperar os registros do cliente – session.get(Customer.class, 1)

Segunda instrução Select para recuperar suas coleções relacionadas – quando iteramos endereços usando o loop for aprimorado



## FetchMode SELECT com tamanho do lote

Nenhuma alteração no xml acima, pois já especificamos o tamanho do lote como 10

(ou)

Nenhuma alteração na classe anotada acima, pois já especificamos o tamanho do lote como 10

Buscar até “ N ” coleções ou entidades (“Não número de registros”)

O maior equívoco desse modo de busca é que o tamanho do lote corresponde ao número de registros buscados por coleção. 
Isso é um completo mal-entendido. O tamanho do lote decide o número de coleções a serem carregadas quando carregamos uma entidade.

Exemplo:

Demos o tamanho do lote como “ 10 ”

Agora quando carregamos um Customer , o Hibernate carrega a coleção de endereços para 10 Customers adicionais que estão atualmente na sessão.

Isso significa que 10 coleções de endereços são carregadas uma para cada Cliente.

Suponha que temos 20 clientes na sessão e o tamanho do lote é definido como 10

Neste caso, ao carregarmos um Cliente, serão executadas 3 consultas

1. Uma consulta para carregar todos os 20 clientes.

2. Uma consulta para carregar as coleções de endereços para 10 clientes.

3. Outra consulta para carregar as coleções de endereços para outros 10 clientes

Se tivermos apenas um usuário , as consultas geradas com o tamanho do lote serão as mesmas que sem o tamanho do lote conforme abaixo

Consultas geradas pelo Hibernate


Hibernar: 

    selecione
 
        customer0_.Id como Id1_1_0_, 

        customer0_.FirstName como FirstNam2_1_0_, 

        customer0_.LastName como LastName3_1_0_, 

        customer0_.City como City4_1_0_    dos 
        clientes customer0_ onde 
        customer0_.Id=?

 


   




Hibernar: 

    selecione
 
        endereços0_.CUSTOMER_ID como CUSTOMER5_0_1_, 

        endereços0_.Id como Id1_0_1_, 

        endereços0_.idx como idx6_1_, 

        endereços0_.Id como Id1_0_0_, endereços0_.Street 
        como 

        Street2_0_0_, 

        endereços0_.Landmark como Landmark3_0_0_, 

        endereços0_.Código_Custo como PostalCo4_0_0_0_0_.   do 
        endereço address0_   onde 
        address0_.CUSTOMER_ID=?


  
Suponha que temos 10 usuários e carregamos todos eles usando a consulta abaixo

List<Customer> customers = session.createQuery("from Customer").getResultList();
    for (Customer customer : customers) {
                       List<Address> addresses = customer.getAddresses();
                    for (Address address : addresses) {
                        System.out.println(address);
                    }  
                   
                }
                
  Agora veja as consultas geradas pelo Hibernate com tamanho de lote(Batch ) de 10              
 
  
  
Hibernate:

    select

        customer0_.Id as Id1_1_,

        customer0_.FirstName as FirstNam2_1_,

        customer0_.LastName as LastName3_1_,

        customer0_.City as City4_1_ 

    from

        customers customer0_



Hibernate: 

    select

        addresses0_.CUSTOMER_ID as CUSTOMER5_0_1_,

        addresses0_.Id as Id1_0_1_,

        addresses0_.idx as idx6_1_,

        addresses0_.Id as Id1_0_0_,

        addresses0_.Street as Street2_0_0_,

        addresses0_.Landmark as Landmark3_0_0_,

        addresses0_.PostalCode as PostalCo4_0_0_,

        addresses0_.CUSTOMER_ID as CUSTOMER5_0_0_ 

    from

        address addresses0_ 

    where

        addresses0_.CUSTOMER_ID in (
            ?, ?, ?,?,?,?,?,?,?,?
        )
        
  Agora o Hibernate gerou 2 consultas

Primeiro Selecione a consulta para recuperar todos os registros do cliente

Segunda consulta Select com “ IN ” para recuperar sua coleção de endereços para 10 clientes (podemos ver 10 símbolos '?').

Se não usarmos o tamanho do lote neste caso, terminaremos com N+1 consultas onde “ N ” corresponde ao número de registros do Cliente na sessão que é 10 neste exemplo.

Assim, terminaremos com 10+1 = 11 consultas.

1 consulta para recuperar todos os 10 clientes
1 consulta para recuperar seus endereços para o 1º cliente
1 consulta para recuperar seu endereço para o 2º cliente
E assim sucessivamente até 10 clientes

No caso, se tivermos 20 clientes na sessão e o tamanho do lote for 10

Então o Hibernate irá gerar 3 consultas

1.Selecione a consulta para recuperar todos os registros do cliente
2.Selecione a consulta com “ IN ” para recuperar sua coleção de endereços para os 10 primeiros clientes.
3.selecione a consulta com “ IN ” para recuperar sua coleção de endereços dos últimos 10 clientes

Observação: a estratégia de busca de tamanho de lote não especifica quantos registros nas coleções são carregados. Em vez disso, especifica quantas coleções devem ser carregadas por vez.


## FetchMode SUBSELECT(SUBSELECT do Modo de Busca)

Modifique o xml acima com fetch=”subselect”

(ou)

Modifique a classe anotada acima com @Fetch(FetchMode.SUBSELECT)

Agrupe a coleção de uma entidade em uma consulta Sub-Select.

Esta estratégia carrega as coleções usando uma subconsulta

Suponha que temos 10 usuários e carregamos todos eles usando a consulta abaixo


List<Customer> customers = session.createQuery("from Customer").getResultList();
	for (Customer customer : customers) {
	        		   List<Address> addresses = customer.getAddresses();
	   				for (Address address : addresses) {
	   					System.out.println(address);
	   				}	
					
				}
        
Hibernate generates 2 queries in this case

Hibernate: 

    select

        customer0_.Id as Id1_1_,

        customer0_.FirstName as FirstNam2_1_,

        customer0_.LastName as LastName3_1_,

        customer0_.City as City4_1_ 

    from

        customers customer0_


Hibernate: 

    select

        addresses0_.CUSTOMER_ID as CUSTOMER5_0_1_,

        addresses0_.Id as Id1_0_1_,

        addresses0_.idx as idx6_1_,

        addresses0_.Id as Id1_0_0_,

        addresses0_.Street as Street2_0_0_,

        addresses0_.Landmark as Landmark3_0_0_,

        addresses0_.PostalCode as PostalCo4_0_0_,

        addresses0_.CUSTOMER_ID as CUSTOMER5_0_0_ 

    from

        address addresses0_ 

    where

        addresses0_.CUSTOMER_ID in (

            select

                customer0_.Id 

            from

                customers customer0_
        )
        
A primeira consulta de seleção é para recuperar todos os clientes A
segunda consulta de seleção é para selecionar as coleções de endereços de todos os clientes usando uma subconsulta.

Mesmo que tenhamos 30 clientes, as mesmas 2 consultas serão geradas.

## Qual modo de busca devo usar?

A pergunta mais comum é “ Qual é a melhor estratégia de busca para usar no meu aplicativo ”

A resposta a esta pergunta depende de muitos fatores, como aplicativo , relacionamento de entidade , etc.

Vamos ver algumas das orientações gerais para escolher o melhor


- FetchMode JOIN

Nesse caso, sabemos que os dados serão carregados antes mesmo de usá-los, mas isso cria um número mínimo de consultas.

Às vezes , o Single Join é mais rápido do que vários Selects , mas o Join não será uma boa escolha se envolver muitos dados .

Podemos preferir este modo sempre que tivermos menos dados nas coleções .

Será mais rápido ao acessar as coleções, pois já carregou tudo de uma só vez.

- FetchMode SELECT

Isso é melhor para usar quando queremos uma resposta mais rápida ao acessar uma única entidade.

Ele carrega dados adicionais somente quando necessário .

Exemplo:
Usuário com lista de endereços

Em muitos lugares do nosso aplicativo, podemos precisar apenas dos detalhes do usuário e não dos detalhes do endereço

Nesse caso, carregar apenas os detalhes do usuário obterá uma resposta mais rápida .

Sempre que precisarmos de endereços de um usuário em outras partes de um aplicativo, ele será carregado apenas naquele momento.


- FetchMode SELECT with BatchSize

BatchSize é útil quando se tem um conjunto fixo de dados .

Exemplo:
Quando temos um processamento em lote de, digamos, 10 usuários por vez.

BatchSize de 10 reduzirá drasticamente o número de consultas necessárias.

O ponto mais importante a ser observado aqui é que, se o tamanho do lote for pequeno , seu desempenho será bom , caso contrário, levará mais tempo e afetará o desempenho .

Temos que preferir esse modo quando temos um tamanho de lote pequeno e temos a necessidade de processar os dados em lote.



- FetchMode SUBSELECT

Este modo buscará todas as coleções relacionadas em uma subconsulta.

Isso deve ser usado quando temos uma entidade onde a maioria delas são carregadas na sessão .

O ponto mais importante a ser observado aqui é que todas as coleções são buscadas mesmo se o pai não estiver na sessão .

Exemplo:
Se temos 100 usuários no banco de dados e apenas alguns usuários estão na sessão , usar SubSelect não é uma boa escolha , pois carrega os endereços de todos os 100 usuários usando a subconsulta.

Devemos preferir quando temos uma entidade cuja maioria dos registros já estão carregados na sessão.

Nota:
Usar a estratégia de busca correta com base em nosso requisito é importante para otimizar o desempenho da consulta do Hibernate.
Se usarmos essas estratégias de maneira errada, isso afetará gravemente o desempenho da consulta.
Portanto, use a estratégia certa com uma compreensão completa do conceito e do requisito.

## Referencias

https://examples.javacodegeeks.com/enterprise-java/hibernate/hibernate-transaction-example/
http://javainsimpleway.com/hibernate-fetch-types/

