# Cache

6.1. O cache de consulta
Se você tiver consultas que são executadas repetidamente, com os mesmos parâmetros, o cache de consultas fornece ganhos de desempenho.

O armazenamento em cache introduz sobrecarga na área de processamento transacional. Por exemplo, se você armazenar em cache os resultados de uma consulta em
um objeto, o Hibernate precisa acompanhar se alguma alteração foi confirmada no objeto e invalidar o cache adequadamente. Além disso, o benefício de armazenar 
em cache os resultados da consulta é limitado e altamente dependente dos padrões de uso do seu aplicativo. Por esses motivos, o Hibernate desabilita o cache
de consulta por padrão.

##  Query cache regions

Example 6.1. Method setCacheRegion

List blogs = sess.createQuery("from Blog blog where blog.blogger = :blogger")
        .setEntity("blogger", blogger)
        .setMaxResults(15)
        .setCacheable(true)
        .setCacheRegion("frontpages")
        .list();
        
        
