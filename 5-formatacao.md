# Formatação

Ao escrevermos nosso codigo devemos querer que as pessoas o leia e perceba que é um código bem cuidado, que foi feito por um profissional. Se, ao invés disso ele encontrar um monte de sujeira e coisas fora do padrão, ele irá concluir que o projeto é uma bagunça.

Ao trabalhar com código, é importante que haja uma guideline consistente de formatação, e assegurar que essa mesma guideline é seguida pelo time como um todo.

## O propósito da formatação

O principal propósito é a **comunicação**, o que deve ser a principal preocupação de um bom desenvolvedor. Só estar funcionando não é o bastante, após inúmeras manutenções, são o estilo e a disciplina do código que permanecem, apesar de o código original ter ido embora.

## Formatação Vertical

Qual deve ser o tamanho ideal de um código fonte?

Após análises de projetos open-source, foi determinado que é possível desenvolver projetos significativos (50,000 linhas) a partir de arquivos entre 200 e 500 linhas.

Lembre-se que, arquivos pequenos normalmente são mais fáceis de ler e entender do que arquivos grandes.

## Metáfora do Jornal

Pense num artigo de jornal bem escrito. No topo se espera um nome pelo qual você saiba sobre o que é a história e, logo, se você tem interesse nisso ou não. Ao descer é possível identificar uma pequena sinopse, e quanto mais você lê, mais informações sobre aquele assunto você absorve.

E assim deve ser um código-fonte, queremos que seu nome seja descritivo o suficiente para sabermos se o que buscamos se encontra nele ou não. E no decorrer da leitura, quanto mais lemos o arquivo, mais específicas vão ficando os detalhes das informações.

Um jornal é composto de vários artigos, uns grandes, outros pequenos. Entretanto, se o jornal fosse apenas uma longa história composta por um aglomerado de fatos, nomes e datas desorganizados, seria simplesmente impossível de lê-lo.

## Aberturas na Vertical

Ao mudar de conceito, adicione uma linha em branco para aumentar a legibilidade.

```java
package fitnesse.wikitext.widgets;
import java.util.regex.*;
public class BoldWidget extends ParentWidget {
    public static final String REGEXP = "'''.+?'''";
    private static final Pattern pattern = Pattern.compile"'''(.+?)'''",
        Pattern.MULTILINE + Pattern.DOTALL
        ); 
    public BoldWidget(ParentWidget parent, String text) throws Exception {
        super(parent);
        Matcher match = pattern.matcher(text);
        match.find();
        addChildWidgets(match.group(1));}
    public String render() throws Exception {
        StringBuffer html = new StringBuffer("<b>");
        html.append(childHtml()).append("</b>");
        return html.toString();
    }
}
```
Deixar muito amontoado prejudica a leitura.

```java
package fitnesse.wikitext.widgets;

import java.util.regex.*;

public class BoldWidget extends ParentWidget {

    public static final String REGEXP = "'''.+?'''";
    private static final Pattern pattern = Pattern.compile"'''(.+?)'''",
        Pattern.MULTILINE + Pattern.DOTALL
        ); 

    public BoldWidget(ParentWidget parent, String text) throws Exception {
        super(parent);
        Matcher match = pattern.matcher(text);
        match.find();
        addChildWidgets(match.group(1));
    }

    public String render() throws Exception {
        StringBuffer html = new StringBuffer("<b>");
        html.append(childHtml()).append("</b>");
        return html.toString();
    }

}
```
Ao dividirmos com espaços fica muito melhor organizado.

## Densidade Vertical

Ao pularmos linha, separamos diferentes conceitos, entretanto, linhas que estão juntas implicam em associação.
```java
public class ReporterConfig {

    /**
     * The class name of the reporter listener
     */
    private String m_className;

    /**
     * The properties of the reporter listener
     */
    private List<Property> m_properties = new ArrayList<Property>();

    public void addProperty(Property property) {
      m_properties.add(property);
    }
}
```
A distância e a adição de linhas desnecessárias prejudicam a leitura.

```java
public class ReporterConfig {

    private String m_className;
    private List<Property> m_properties = new ArrayList<Property>(); 

    public void addProperty(Property property) {
      m_properties.add(property);
    }
}
```
Ao retirarmos os comentários, a associação é feita e a legibilidade é aumentada.

## Distância Vertical

### Variáveis Locais

```java
private static void readPreferences() {
    InputStream is = null;
    try {
       is= new FileInputStream(getPreferencesFile());
       setPreferences(new Properties(getPreferences()));
       getPreferences().load(is);
    } catch (IOException e) {
       try {
         if (is != null)
           is.close();
       } catch (IOException e1) {
       }
    }
}
```
Devem ser declaradas mais perto do seu uso quanto possível. Portanto, ao utilizarmos funções pequenas, as variáveis locais devem ser declaradas no topo da função.

```java
public int countTestCases() {
    int count= 0;
    for (Test each : tests)
       count += each.countTestCases();
    return count;
}
```
Variáveis de controle para loops devem ser declaradas junto com seus loops.

```java
public int countTestCases() {
    int count = tests.stream()
        .reduce(0, this::countTestCases)
    return count;
}
```
//**Update pessoal**: encorajo aqui o uso de Streams do Java 8 para fazer essa iteração

### Variáveis de Instância

```java
public class ReporterConfig {

    private String m_className;
    private List<Property> m_properties = new ArrayList<Property>(); 

    public void addProperty(Property property) {
      m_properties.add(property);
    }

    public void getClassName() {
      return m_className;
    }
}
```

Por convenção devem ser declaradads no topo da classe. Numa classe bem desenhada, as variáveis de instância são utilizadas por muitos métodos, o que não altera a distância vertical.

```java
public class ReporterConfig {
    private List<Property> m_properties = new ArrayList<Property>(); 

    public void addProperty(Property property) {
      m_properties.add(property);
    }

    private String m_className;

    public void getClassName() {
      return m_className;
    }
}
```

Ao declarar essas variáveis em qualquer outro lugar, a legibilidade do código é prejudicada.

### Funções Dependentes

Se uma função chama a outra, elas devem ser verticalmente próximas, e a função que chama deve vir logo antes da função chamada, se possível.

### Afinidade Conceitual

Quanto maior a afinidade, menor a distância vertical que deve haver entre os bits. Além dos exemplos anteriores, a afinidade pode ser causada devido a um grupo de funções que exercem uma operação similar.

## Formatação Horizontal

Quão extensa deve ser uma linha de código?

Após análises de projetos open-source, foi determinado que o limite ideal de caracteres é 120.

## Densidade e Abertura Horizontal

```java
public class Quadratic {
     public static double root1(double a, double b, double c) {
       double determinant = determinant(a, b, c);
       return (-b + Math.sqrt(determinant)) / (2*a);
     }

     public static double root2(int a, int b, int c) {
       double determinant = determinant(a, b, c);
       return (-b - Math.sqrt(determinant)) / (2*a);
     }

     private static double determinant(double a, double b, double c) {
       return b*b - 4*a*c;
     }
}

private void measureLine(String line) {
    lineCount++;
    int lineSize = line.length();
    totalChars += lineSize;
    lineWidthHistogram.addLine(lineSize, lineCount);
    recordWidestLine(lineSize);
}
```

A ideia é adicionar os espaços nos locais corretos, veja a separação entre os operadores e as variáveis. Atente-se também na diferença entre o símbolo de vezes e o de subtração do determinante. São pequenos detalhes que aumentam a legibilidade do código.

## Alinhamento Horizontal

```java
public class FitNesseExpediter implements ResponseSender
   {
     private   Socket          socket;
     private   InputStream     input;
     private   OutputStream    output;
     private   Request         request;
     private   Response        response;
     private   FitNesseContext context;
     protected long            requestParsingTimeLimit;
     private   long            requestProgress;
     private   long            requestParsingDeadline;
     private   boolean         hasError;

     public FitNesseExpediter(Socket          s, 
                              FitNesseContext context) throws Exception
     {
       this.context =            context;
       socket =                  s;
       input =                   s.getInputStream();
       output =                  s.getOutputStream();
       requestParsingTimeLimit = 10000;
     }
}
```
Este tipo de alinhamento pode parecer bonito, mas não é útil no momento em que pode acabar enfatizando o nome da variável e deixando seu tipo em segundo plano, ou acabar não prestando atenção no operador.

No final das contas, toda ferramenta de formatação automática acaba com esse alinhamento. Portanto, procure usar o "normal".

```java
public class FitNesseExpediter implements ResponseSender
   {
     private Socket socket;
     private InputStream input;
     private OutputStream output;
     private Request request;

     private Response response;
     private FitNesseContext context;
     protected long requestParsingTimeLimit;
     private long requestProgress;
     private long requestParsingDeadline;
     private boolean hasError;

     public FitNesseExpediter(Socket s, FitNesseContext context) throws Exception
     {
       this.context = context;
       socket = s;
       input = s.getInputStream();
       output = s.getOutputStream();
       requestParsingTimeLimit = 10000;
     }
```

## Identação

Utilize identação para melhorar a legibilidade.

Nunca quebre a identação.

## Regras do Time

É de grande importância que um software possua um estilo de formatação conciso e consistente. Portanto, é de extrema importância que haja um consenso em onde colocar chaves, tamanho da identação, nomenclatura de classes, variaveis, funções e etc... Isso pode ser feito criando-se uma guideline.

Exemplos de Guideline:
- [Google](https://google.github.io/styleguide/javaguide.html)
- [Twitter](https://github.com/twitter/commons/blob/master/src/java/com/twitter/common/styleguide.md)