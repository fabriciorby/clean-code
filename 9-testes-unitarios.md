# Testes Unitários

Antigamente as pessoas nunca tinham ouvido falar de TDD (Test Driven Development) e nõa davam importância para os testes unitários. Ou elas faziam apenas por fazer, ou simplesmente não faziam. Não havia muita exigência de qualidade. Graças ao Agile e ao TDD, muitos desenevolvedores estão se tornando adeptos a escrever testes unitários de qualidade.

## As três regras do TDD

1. Você não deve escrever código de produção até que você tenha escrito um teste unitário de falha.
2. Você não deve escrever mais de um teste unitário que seja suficiente para falhar, e 'não compilar' é falhar.
3. Você não deve escrever mais código de produção do que o suficiente para passar o teste que está falhando.

Seguindo essas regras entramos em um ciclo onde os testes e o código de produção são escritos *juntos*, com os testes sendo escritos segundos antes do código de produção.

Ao trabalhar dessa forma, os testes irão cobrir todo o código de produção possível.

## Mantendo os Testes Limpos

Pense no seguinte: Era uma vez uma equipe de engenharia de uma empresa, que decidiu que fazer testes é obrigatório, mas que não dá a mínima para a qualidade do seu código de teste unitário, fazendo tudo de qualquer jeito, de forma que o código de produção atual passe.

Entretanto, o que essa equipe não percebeu é que ter testes sujos é equivalente a não ter testes  (ou pior). O problema é que os testes devem ser modificados assim que o código de produção evolui, quanto mais sujo o teste, mais difícil a modificação. Chega um momento em que é mais demorado desenvolver os testes do que o próprio código de produção. Ao modificar o código de produção, testes velhos começam a falhar e, devido a bagunça, é difícil fazer esses testes passarem novamente.

Essa dificuldade com os testes tende a piorar com o passar do tempo até se tornar insustentável, e a única forma de seguir em frente é reescrever todos os testes ou simplesmente ignorá-los. Ao escolher o caminho mais fácil não é possível saber se uma mudança no código de produção pode ter acarretado em algum problema nas funções já existentes e, caso não tenha, não é possível refatorar o código para deixá-lo mais limpo, simplesmente por medo de quebrar algo durante o processo.

> O código do teste unitário é tão importante quanto o código de produção.

## A importância dos Testes

O Teste Unitário é o que mantém o código de produção flexível, manutenível e reusável. Se você tem testes você não tem medo de modificar o código, sem testes cada alteração é um possível bug.

> Testes habilitam mudanças.

## Testes Limpos

O que faz um teste ser limpo?

*Legibilidade*.

Listing 9-1 SerializedPageResponderTest.java
```java
public void testGetPageHieratchyAsXml() throws Exception {
    
     crawler.addPage(root, PathParser.parse("PageOne"));
     crawler.addPage(root, PathParser.parse("PageOne.ChildOne"));
     crawler.addPage(root, PathParser.parse("PageTwo"));

     request.setResource("root");
     request.addInput("type", "pages");

     Responder responder = new SerializedPageResponder();
     SimpleResponse response =
       (SimpleResponse) responder.makeResponse(
          new FitNesseContext(root), request);
     String xml = response.getContent();

     assertEquals("text/xml", response.getContentType());

     crawler.addPage(root, PathParser.parse("PageOne"));
     crawler.addPage(root, PathParser.parse("PageOne.ChildOne"));
     crawler.addPage(root, PathParser.parse("PageTwo"));

     request.setResource("root");
     request.addInput("type", "pages");

     Responder responder = new SerializedPageResponder();
     SimpleResponse response =
       (SimpleResponse) responder.makeResponse(
          new FitNesseContext(root), request);
     String xml = response.getContent();

     assertEquals("text/xml", response.getContentType());
     assertSubString("<name>PageOne</name>", xml);
     assertSubString("<name>PageTwo</name>", xml);
     assertSubString("<name>ChildOne</name>", xml);
   }

public void testGetPageHieratchyAsXmlDoesntContainSymbolicLinks() throws Exception {

     WikiPage pageOne = crawler.addPage(root, PathParser.parse("PageOne"));
     crawler.addPage(root, PathParser.parse("PageOne.ChildOne"));
     crawler.addPage(root, PathParser.parse("PageTwo"));

     PageData data = pageOne.getData();
     WikiPageProperties properties = data.getProperties();

     crawler.addPage(root, PathParser.parse("PageOne"));
     crawler.addPage(root, PathParser.parse("PageOne.ChildOne"));
     crawler.addPage(root, PathParser.parse("PageTwo"));

     request.setResource("root");
     request.addInput("type", "pages");

     Responder responder = new SerializedPageResponder();
     SimpleResponse response =
       (SimpleResponse) responder.makeResponse(
          new FitNesseContext(root), request);
     String xml = response.getContent();

     assertEquals("text/xml", response.getContentType());
     assertSubString("<name>PageOne</name>", xml);
     assertSubString("<name>PageTwo</name>", xml);
     assertSubString("<name>ChildOne</name>", xml);
   }

public void testGetPageHieratchyAsXmlDoesntContainSymbolicLinks() throws Exception {

     WikiPage pageOne = crawler.addPage(root, PathParser.parse("PageOne"));
     crawler.addPage(root, PathParser.parse("PageOne.ChildOne"));
     crawler.addPage(root, PathParser.parse("PageTwo"));

     PageData data = pageOne.getData();
     WikiPageProperties properties = data.getProperties();
     WikiPageProperty symLinks = properties.set(SymbolicPage.PROPERTY_NAME);
     symLinks.set("SymPage", "PageTwo");
     pageOne.commit(data);

     request.setResource("root");
     request.addInput("type", "pages");
     Responder responder = new SerializedPageResponder();
     SimpleResponse response =
       (SimpleResponse) responder.makeResponse(
          new FitNesseContext(root), request);
     String xml = response.getContent();

     assertEquals("text/xml", response.getContentType());
     assertSubString("<name>PageOne</name>", xml);
     assertSubString("<name>PageTwo</name>", xml);
     assertSubString("<name>ChildOne</name>", xml);
     assertNotSubString("SymPage", xml);
   }

public void testGetDataAsHtml() throws Exception {

     crawler.addPage(root, PathParser.parse("TestPageOne"), "test page");

     request.setResource("TestPageOne");
     request.addInput("type", "data"); 
     Responder responder = new SerializedPageResponder();
     SimpleResponse response =
       (SimpleResponse) responder.makeResponse(
          new FitNesseContext(root), request);
     String xml = response.getContent();

     assertEquals("text/xml", response.getContentType());
     assertSubString("test page", xml);
     assertSubString("<Test", xml);
   }
```

Listing 9-2 SerializedPageResponderTest.java (refactored)
```java
public void testGetPageHierarchyAsXml() throws Exception {
     makePages("PageOne", "PageOne.ChildOne", "PageTwo");

     submitRequest("root", "type:pages");

     assertResponseIsXML();
     assertResponseContains("<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>"
     );
   }

public void testSymbolicLinksAreNotInXmlPageHierarchy() throws Exception {
     WikiPage page = makePage("PageOne");
     makePages("PageOne.ChildOne", "PageTwo");

     addLinkTo(page, "PageTwo", "SymPage");

     submitRequest("root", "type:pages");

     assertResponseIsXML();
     assertResponseContains(
       "<name>PageOne</name>", "<name>PageTwo</name>",
              "<name>ChildOne</name>"
     );
     assertResponseDoesNotContain("SymPage");
   } 

public void testGetDataAsXml() throws Exception {
     makePageWithContent("TestPageOne", "test page");

     submitRequest("TestPageOne", "type:data");

     assertResponseIsXML();
     assertResponseContains("test page", "<Test");
   }
```

Com a refatoração, a pattern *BULD-OPERATE-CHECK* ficou evidente.

A primeira parte builda a data do teste, a segunda parte executa as operações e a terceira checa o resultado.

Procure utilizar apenas um assert por teste, e teste um conceito de cada vez.

## Regras F.I.R.S.T

* Fast: Testes devem ser rápidos para que possam ser rodados com frequência.
* Independent: Testes não devem depender de outros testes, testes devem poder rodar independentemente e em qualquer ordem desejada.
* Repeatable: Testes devem poder ser repetidos em qualquer ambiente. (produção/homologação/desenvolvimento/local)
* Self-Validating: Devem retornar um output booleano. Pass ou Fail.
* Timely: Os testes devem ser escritos momentos antes do código de produção que faz ele passar.

## Conclusão
Testes são tão importantes quanto o código de produção. Se você deixar os testes apodrecerem, o código de produção também irá apodrecer.