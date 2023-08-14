![image](https://github.com/filhotecmail/Execute_block_Return-/assets/18727307/1e9243ff-461b-4dd4-92ba-fd2c498d9e02)

# On the Fly 

"Execute Block - on-the-fly" é um jeito bacana de executar códigos de banco de dados de forma dinâmica. É como se a gente escrevesse um conjunto de instruções em um bloco especial chamado "EXECUTE BLOCK", que depois é executado. A parte legal é que a gente não precisa criar algo já definido, como um plano ou uma receita. É ótimo para fazer tarefas específicas que não são complexas o suficiente para criar um "plano" grande, mas que ainda precisam ser feitas.

Pense nisso como uma forma de fazer um script momentâneo, como uma listinha de coisas para fazer, só que diretamente no banco de dados. Assim, dá para executar rapidinho algumas ações sem criar algo fixo e permanente.

Imagine que você está dizendo para o banco de dados: "Ei, vou escrever um bloquinho de coisas para você fazer. Vou te dar as instruções e você executa tudo sem precisar guardar essas instruções para sempre".

Esse bloquinho é bem fácil de usar, você coloca as instruções dentro do bloco usando as palavras mágicas "BEGIN" (começar) e "END" (terminar). Pode incluir coisas como dizer o que selecionar, inserir, atualizar ou apagar. Você até pode cuidar de situações inesperadas, como quando algo não dá certo, tudo isso dentro desse bloquinho.

Falando de uma forma mais científica aplicada a modelos computacionais , pode-se subentender que o compilador irá executar esses médodos isolados como uma sub-routine, e manterá a ordem cornológica top-down de execução.
Isso é alinhado estaticamente a um único ponteiro para que a ordem de execução seja garantida e executada por uma chamada anônima, em Thread, isolada para não concorrer com outras requisições.

Você não precisa executar literalmente "In Autonomous Transaction", para que isso seja executado.

# Aplicabilidade
  Posso ilustrar vários cenários de aplicabilidade na vida real para problemas trasncritos para modelos computacionais, dentre eles estão ligados problemas dinâmicos, aqueles que não dependem que explicitamente estejam sendo informados por procedimentos armazenados.
Como por exemplo, comitar uma venda , e executar diversas instruções de tratamentos e retornar dados consistentes e transacionados.
Outro exemplo, é inferir parametro do tipo blob text em formato de arquivo, bytes ou hexadecimal e inserir um conjunto arbitrário de dados simultaneamente, isolado , consistente e concomitante, tal técnica é conhecida como Array DML statement.

# Execute block with Return
  
  Execução de Return Block com Retorno de resultado - SQL

O tema é útil em casos onde é necessário realizar alterações a partir de seleções complexas, o que demandaria a criação de uma Procedure, trazendo consigo a vantagem de executar qualquer comando presente em PSQL de forma simplificada, sem a necessidade de ser efetivamente criado no banco de dados.

Executa um bloco de código PSQL como se fosse um procedimento armazenado, opcionalmente com parâmetros de entrada e saída e declarações variáveis. Isso permite que o usuário execute psql "on-the-fly" dentro de um contexto DSQL;

Os Procedimentos de Execução instantânea em linha são executados e podem quando em SQL sem a nececessidade de escrever uma Procedural SQL / P-SQL não precisa ser Armazenado.

ex.:

```SQL

EXECUTE BLOCK [(<inparams>)]
     [RETURNS (<outparams>)]
AS
   [<declarations>]
BEGIN
   [<PSQL statements>]
END

<inparams>      ::=  paramname type = ? [, <inparams>]
<outparams>     ::=  paramname type [, <outparams>]
<declarations>  ::=  See PSQL::DECLARE for the exact syntax

```

Nesse exemplo ilustrado abaixo , eu Insiro um novo produto na tabela, ligando ao numero da Venda e calculo a sua posição no cupom, para atender a numeração de ordem por cupom.

veja 


![image](https://user-images.githubusercontent.com/18727307/192142610-15a16ce6-e5ee-40cd-b2a4-5f2812d7ca55.png)


![image](https://user-images.githubusercontent.com/18727307/192142675-5d1d815c-8bb9-40d2-bae7-3a8db3319526.png)

Perceba a coluna H02NITEM, elarecebe a ordem de adição de acordo com a venda , na Venda Nroº 128 o item adicionado foi o numero 7
 porém vou inserir mais 2 itens em uma outra venda que não tem itens na tabela.

![image](https://user-images.githubusercontent.com/18727307/192142746-f32b1980-d85c-431c-9edc-9c30fe9b2be5.png)

 


```SQL

EXECUTE BLOCK (

    I001AIDPK BIGINT = :I001AIDPK,
    I02CPROD VARCHAR(60) = :I02CPROD,
    I03CEAN VARCHAR(14) = :I03CEAN,
    I04XPROD VARCHAR(120) = :I04XPROD,
    I05NCM VARCHAR(8) = :I05NCM,
    I08CFOP VARCHAR(4) = :I08CFOP,
    I09UCOM VARCHAR(6) = :I09UCOM,
    I10QCOM NUMERIC(15,4) = :I10QCOM,
    I10AVUNCOM NUMERIC(15,2) = :I10AVUNCOM,
    I11VPROD NUMERIC(15,2) = :I11VPROD,
    I12CEANTRIB VARCHAR(14) = :I12CEANTRIB,
    I13UTRIB VARCHAR(6) = :I13UTRIB,
    I14QTRIB NUMERIC(15,4) = :I14QTRIB,
    I14AVUNTRIB NUMERIC(15,2) = :I14AVUNTRIB,
    I15VFRETE NUMERIC(15,2) = :I15VFRETE,
    I16VSEG NUMERIC(15,2) = :I16VSEG,
    I17VDESC NUMERIC(15,2) = :I17VDESC,
    I17AVOUTRO NUMERIC(15,2) = :I17AVOUTRO,
    I17BINDTOT INTEGER = :I17BINDTOT,
    I05CCEST VARCHAR(7) = :I05CCEST
   )
 RETURNS ( LID BIGINT,
           LI001AIDPK BIGINT,
           LH02NITEM INTEGER,
           LI02CPROD VARCHAR(60),
           LI03CEAN VARCHAR(14),
           LI04XPROD VARCHAR(120),
           LI05NCM VARCHAR(8),
           LI08CFOP VARCHAR(4),
           LI09UCOM VARCHAR(6),
           LI10QCOM NUMERIC(15,4),
           LI10AVUNCOM NUMERIC(15,2),
           LI11VPROD NUMERIC(15,2),
           LI12CEANTRIB VARCHAR(14),
           LI13UTRIB VARCHAR(6),
           LI14QTRIB NUMERIC(15,4),
           LI14AVUNTRIB NUMERIC(15,2),
           LI15VFRETE NUMERIC(15,2),
           LI16VSEG NUMERIC(15,2),
           LI17VDESC NUMERIC(15,2),
           LI17AVOUTRO NUMERIC(15,2),
           LI17BINDTOT INTEGER,
           LI05CCEST VARCHAR(7) )
 AS
  DECLARE VARIABLE LINCPOSITION SMALLINT;
  DECLARE VARIABLE LRETURNPOSITION SMALLINT;
BEGIN

 LINCPOSITION = ( SELECT
                       CAST(COUNT( ID ) + 1 AS SMALLINT) AS LPOSITION
                       FROM PROD P  WHERE P.I001AIDPK = :I001AIDPK);

 LRETURNPOSITION =  ( SELECT
                      CAST(COUNT( ID ) AS SMALLINT) AS LPOSITION
                      FROM PROD P WHERE P.I001AIDPK = :I001AIDPK );

 INSERT INTO PROD ( I001AIDPK,H02NITEM, I02CPROD, I03CEAN, I04XPROD, I05NCM,
                    I08CFOP, I09UCOM, I10QCOM, I10AVUNCOM, I11VPROD, I12CEANTRIB,
                    I13UTRIB, I14QTRIB, I14AVUNTRIB, I15VFRETE, I16VSEG,
                    I17VDESC, I17AVOUTRO, I17BINDTOT, I05CCEST )

  VALUES (:I001AIDPK,:LINCPOSITION, :I02CPROD, :I03CEAN, :I04XPROD,
          :I05NCM, :I08CFOP, :I09UCOM, :I10QCOM, :I10AVUNCOM,
          :I11VPROD, :I12CEANTRIB, :I13UTRIB, :I14QTRIB, :I14AVUNTRIB,
          :I15VFRETE, :I16VSEG, :I17VDESC, :I17AVOUTRO, :I17BINDTOT, :I05CCEST)

  RETURNING ID,
            I001AIDPK,
                  :LRETURNPOSITION,
                  I02CPROD, I03CEAN, I04XPROD, I05NCM, I08CFOP, I09UCOM,
                  I10QCOM, I10AVUNCOM, I11VPROD, I12CEANTRIB, I13UTRIB,
                  I14QTRIB, I14AVUNTRIB, I15VFRETE, I16VSEG, I17VDESC, I17AVOUTRO,
                  I17BINDTOT, I05CCEST
  INTO
    :LID,
    :LI001AIDPK,
    :LH02NITEM,
    :LI02CPROD,
    :LI03CEAN,
    :LI04XPROD,
    :LI05NCM,
    :LI08CFOP,
    :LI09UCOM,
    :LI10QCOM,
    :LI10AVUNCOM,
    :LI11VPROD,
    :LI12CEANTRIB,
    :LI13UTRIB,
    :LI14QTRIB,
    :LI14AVUNTRIB,
    :LI15VFRETE,
    :LI16VSEG,
    :LI17VDESC,
    :LI17AVOUTRO,
    :LI17BINDTOT,
    :LI05CCEST;

  SUSPEND;

END







```
