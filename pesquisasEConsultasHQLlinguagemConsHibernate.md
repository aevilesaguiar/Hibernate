# HQL: A linguagem de consulta do Hibernate

15.1. Case Sensitivity
15.2. The from clause (Clausula)
15.3. Associations and joins(Associações e associações)
15.4. Forms of join syntax(Formas de sintaxe de junção)
15.5. Referring to identifier property(Referindo-se à propriedade do identificador)
15.6. The select clause( A cláusula select)
15.7. Aggregate functions(Funções agregadas)
15.8. Polymorphic queries (Consultas polimórficas)
15.9. The where clause (A cláusula where)
15.10. Expressions
15.11. The order by clause(A ordem por cláusula)
15.12. The group by clause(O agrupamento por cláusula)
15.13. Subqueries(Subconsultas)
15.14. HQL examples
15.15. Bulk update and delete (Atualização e exclusão em massa)
15.16. Tips & Tricks(dicas e truques)
15.17. Components
15.18. Row value constructor syntax(Sintaxe do construtor de valor de linha)



O Hibernate usa uma poderosa linguagem de consulta (HQL) com aparência semelhante ao SQL. Comparado com o SQL, no entanto, o HQL 
é totalmente orientado a objetos e entende noções como herança, polimorfismo e associação.


## 15.1. Case Sensitivity( Sensibilidade a maiúsculas e minúsculas)

Com exceção de nomes de classes e propriedades Java, as consultas não diferenciam maiúsculas de minúsculas. 
Então SeLeCTé o mesmo que sELEcté o mesmo que SELECT, mas org.hibernate.eg.FOO não é org.hibernate.eg.Foo, e foo.barSetnão é foo.BARSET.

Este manual usa palavras-chave HQL minúsculas. Alguns usuários acham mais legíveis as consultas com palavras-chave em maiúsculas, mas 
essa convenção não é adequada para consultas incorporadas ao código Java.

## 15.2. The from clause (Clausula)

- A consulta Hibernate mais simples possível é da forma:

from eg.Cat

Isso retorna todas as instâncias da classe eg.Cat. Normalmente, você não precisa qualificar o nome da classe, pois auto-import é o padrão. Por exemplo:

from Cat

- Para fazer referência a Cat a outras partes da consulta, você precisará atribuir um alias . Por exemplo:

from Cat as cat

Essa consulta atribui o alias cat a Cat instâncias, para que você possa usar esse alias posteriormente na consulta. 

A as palavra-chave é opcional. Você também pode escrever:

from Cat cat

- Várias classes podem aparecer, resultando em um produto cartesiano ou junção "cruzada".

from Formula, Parameter
from Formula as form, Parameter as param

É uma boa prática nomear aliases de consulta usando letras minúsculas iniciais, pois isso é consistente com os 
padrões de nomenclatura Java para variáveis locais ((e.g. domesticCat).


## 15.3. Associations and joins(Associações e associações)

Você também pode atribuir aliases a entidades associadas ou a elementos de uma coleção de valores usando um arquivo join. Por exemplo:

from Cat as cat
    inner join cat.mate as mate
    left outer join cat.kittens as kitten
    
from Cat as cat left join cat.mate.kittens as kittens

from Formula form full join form.parameter param

- Os tipos de junção suportados são emprestados do ANSI SQL:

  - inner join

  - left outer join

  - right outer join

  - full join (not usually useful)
  
 As construções inner join, left outer joine right outer joinpodem ser abreviadas.
 
 from Cat as cat
    join cat.mate as mate
    left join cat.kittens as kitten
    
Você pode fornecer condições extras de junção usando a palavra- with chave HQL.

from Cat as cat 
    left join cat.kittens as kitten 
        com kitten.bodyWeight > 10.0
        
Uma junção de "busca" permite que associações ou coleções de valores sejam inicializadas junto com seus objetos pai usando uma única seleção. 
Isso é particularmente útil no caso de uma coleção. Ele efetivamente substitui a associação externa e as declarações lentas do arquivo de mapeamento
para associações e coleções. 

from Cat as cat
    inner join fetch cat.mate
    left join fetch cat.kittens
    
Uma fecth (junção de busca) geralmente não precisa atribuir um alias, porque os objetos associados não devem ser usados  na cláusula where
(ou em qualquer outra cláusula). Os objetos associados também não são retornados diretamente nos resultados da consulta. Em vez disso, 
eles podem ser acessados por meio do objeto pai. A única razão pela qual você pode precisar de um alias é se você estiver reunindo 
recursivamente buscando uma coleção adicional:

from Cat as cat
    inner join fetch cat.mate
    left join fetch cat.kittens child
    left join fetch child.kittens
   
  
  A fetchconstrução não pode ser usada em consultas chamadas using iterate()(embora scroll()possa ser usada). Fetchdeve ser usado junto 
  com setMaxResults()ou setFirstResult(), pois essas operações são baseadas nas linhas de resultado que geralmente contêm duplicatas 
  para busca de coleção antecipada, portanto, o número de linhas não é o que você esperaria. Fetchtambém não deve ser usado em 
  conjunto com improvisado  with condition.  É possível criar um produto cartesiano juntando-se buscando mais de uma coleção em uma 
  consulta, portanto tome cuidado neste caso. A busca de várias funções de coleta de junção pode produzir resultados inesperados
  para mapeamentos de bagagem, portanto, recomenda-se a discrição do usuário ao formular consultas neste caso. Finalmente, 
  observe que full join fetche right join fetch não são significativos.
  
  from Document fetch all properties order by name
  
  from Document doc fetch all properties where lower(doc.name) like '%cats%'
  
  
  

Se você estiver usando o nível de propriedade lazy fetching (com instrumentação de bytecode), é possível forçar o Hibernate a buscar as propriedades 
lazy na primeira consulta imediatamente usando fetch all properties.


## 15.4. Forms of join syntax(Formas de sintaxe de junção)


HQL suporta duas formas de associação de associação: implicite explicit.

Todas as consultas mostradas na seção anterior usam o explicitformulário, ou seja, onde a palavra-chave join é usada explicitamente na cláusula from. Este é o formulário recomendado.

O implicitformulário não usa a palavra-chave join. Em vez disso, as associações são "desreferenciadas" usando notação de ponto. implicitas junções podem aparecer em qualquer uma 
das cláusulas HQL. implicitjoin resultam em associações internas na instrução SQL resultante.


from Cat as cat where cat.mate.name like '%s%'


## 15.5. Referring to identifier property(Referindo-se à propriedade do identificador)

Existem 2 maneiras de se referir à propriedade do identificador de uma entidade:

- A propriedade especial (minúsculas) id pode ser usada para referenciar a propriedade identificadora de uma entidade 
desde que a entidade não defina uma propriedade não identificadora chamada id .

- Se a entidade definir uma propriedade de identificador nomeada, você poderá usar esse nome de propriedade.

As referências às propriedades do identificador composto seguem as mesmas regras de nomenclatura. Se a entidade tiver uma propriedade não identificadora 
denominada id, a propriedade identificadora composta só poderá ser referenciada por seu nome definido. Caso contrário, a idpropriedade especial pode ser 
usada para referenciar a propriedade do identificador.


## 15.6. The select clause( A cláusula select)

A selectcláusula escolhe quais objetos e propriedades devem ser retornados no conjunto de resultados da consulta. Considere o seguinte:

select mate
from Cat as cat
    inner join cat.mate as mate

A consulta selecionará mates de outros Cats. Você pode expressar essa consulta de forma mais compacta como:

select cat.mate from Cat cat

As consultas podem retornar propriedades de qualquer tipo de valor, incluindo propriedades do tipo de componente:

select cat.name from DomesticCat cat
where cat.name like 'fri%'

select cust.name.firstName from Customer as cust

As consultas podem retornar vários objetos e/ou propriedades como uma matriz do tipo Object[]:

select mother, offspr, mate.name
from DomesticCat as mother
    inner join mother.mate as mate
    left outer join mother.kittens as offspr
    
Ou como List:

select new list(mother, offspr, mate.name)
from DomesticCat as mother
    inner join mother.mate as mate
    left outer join mother.kittens as offspr

Ou - supondo que a classe Familytenha um construtor apropriado - como um objeto Java typesafe real:

select new Family(mother, mate, offspr)
from DomesticCat as mother
    join mother.mate as mate
    left join mother.kittens as offspr
    
    

Você pode atribuir aliases a expressões selecionadas usando as:

select max(bodyWeight) as max, min(bodyWeight) as min, count(*) as n
from Cat cat

Você pode atribuir aliases a expressões selecionadas usando as:

select new map( max(bodyWeight) as max, min(bodyWeight) as min, count(*) as n )
from Cat cat

Esta consulta retorna um Map de aliases para valores selecionados.

## 15.7. Aggregate functions
    

## Referencias

https://docs.jboss.org/hibernate/orm/3.5/reference/en/html/queryhql.html
