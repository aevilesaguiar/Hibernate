# Hibernate

## Exemplo de transação de hibernação

Uma transação é uma sequência de operações que funciona como uma unidade atômica. Uma transação só é concluída se todas as operações 
forem concluídas com êxito. Uma transação tem as propriedades de Atomicidade, Consistência, Isolamento e Durabilidade (ACID). 

## Hibernate

- Mapeamento relacional de objeto ou ORM é a técnica de programação para mapear objetos de modelo de domínio de aplicativo para as tabelas de banco de dados relacional
- Hibernate é uma ferramenta ORM baseada em Java que fornece uma estrutura para mapear objetos de domínio de aplicativo para as tabelas de banco de dados relacionais 
e vice-versa. Ele fornece implementação de referência da Java Persistence API, o que o torna uma ótima opção como ferramenta ORM com benefícios de baixo acoplamento
- Um Framework que oferece a opção de mapear objetos Java antigos simples para tabelas de banco de dados tradicionais com o uso de anotações JPA, 
bem como configuração baseada em XML

![image](https://user-images.githubusercontent.com/52088444/183407086-770d278e-6abb-4b54-9814-a6bfacb51182.png)


## Hibernate Transactions(Transações de hibernate)


Uma Transação é uma unidade de trabalho na qual todas as operações devem ser executadas ou nenhuma delas. Para entender a importância da transação, pense em um exemplo 
que se aplica a todos nós, ou seja, Transferência de valor de uma conta para outra, pois esta operação inclui as duas etapas abaixo:

- Deduzir o saldo da conta bancária do remetente
- Adicione o valor à conta bancária do destinatário

![image](https://user-images.githubusercontent.com/52088444/183407538-14f19602-eb09-4073-952b-ad82c4900bbc.png)


Agora pense em uma situação em que o valor é deduzido da conta do remetente, mas não é entregue na conta do destinatário devido a alguns erros. Tais questões são gerenciadas 
pelo gerenciamento de transações onde ambas as etapas são realizadas em uma única unidade. Em caso de falha, a transação deve ser revertida.


OBS: Em tecnologias de banco de dados, um rollback é uma operação que retorna o banco de dados a algum estado anterior. As reversões são importantes para a integridade do banco de dados, 
pois significam que o banco de dados pode ser restaurado para uma cópia limpa mesmo após a execução de operações incorretas

## Propriedades de Transações do Hibernate

Cada transação segue algumas propriedades de transação e estas são chamadas de propriedades ACID . ACID significa Atomicidade , Consistência , Isolamento e Durabilidade .

![image](https://user-images.githubusercontent.com/52088444/183408399-6990c8e8-e937-4657-9599-94603676c3c3.png)

- Atomicidade : é definida como todas as operações podem ser feitas ou todas as operações podem ser desfeitas
- Consistência : Depois que uma transação é concluída com sucesso, os dados no armazenamento de dados devem ser dados confiáveis. Esses dados confiáveis ​​também são chamados de dados consistentes
- Isolamento : Se duas transações estiverem nos mesmos dados, uma transação não perturbará a outra transação
- Durabilidade : Após a conclusão de uma transação, os dados no armazenamento de dados serão permanentes até que outra transação seja realizada nesses dados

## Interface de transações do Hibernate

No framework Hibernate, temos a interface Transaction que define a unidade de trabalho. Ele mantém a abstração da implementação da transação (JTA, JDBC). Uma transação é associada à sessão do 
Hibernate e instanciada chamando o método sessionObj.beginTransaction(). Os métodos da interface de transação são os seguintes:

![image](https://user-images.githubusercontent.com/52088444/183409227-1b6a500f-dc20-4d6b-acd6-2909deed8cf1.png)

## Estrutura Básica do Gerenciamento de Transações do Hibernate

Esta é a estrutura básica que os programas Hibernate devem ter, no que diz respeito ao Tratamento de Transações

Transaction transObj = null;<font></font>
Session sessionObj = null;<font></font>
try {<font></font>
    sessionObj = HibernateUtil.buildSessionFactory().openSession();<font></font>
    transObj = sessionObj.beginTransaction();<font></font>
<font></font>
    //Perform Some Operation Here<font></font>
    transObj.commit();<font></font>
} catch (HibernateException exObj) {<font></font>
    if(transObj!=null){<font></font>
        transObj.rollback();<font></font>
    }<font></font>
    exObj.printStackTrace(); <font></font>
} finally {<font></font>
    sessionObj.close(); <font></font>
}<font></font>


Sempre que a HibernateExceptionacontece chamamos o rollback()método que força o rollback da transação. Isso significa que cada operação dessa transação 
específica que ocorreu antes da exceção será cancelada e o banco de dados retornará ao seu estado anterior a essas operações.

Você pode desenvolver todo o seu sistema sem JPA , apenas com Hibernate ou qualquer outro framework ORM, como o TopLink. Porém você não pode desenvolver o 
sistema apenas com JPA , pois ele é apenas uma interface a ser utilizada por Frameworks ORM.


## Referencias

https://examples.javacodegeeks.com/enterprise-java/hibernate/hibernate-transaction-example/




