# Comentários

>Não comente código ruim, reescreva-o.

O uso mais comum de comentários é para compensar a falha em nos expressar por meio do código.

Comentários mal feitos são piores do que não ter nenhum comentário.

A partir da adição edição de código ou modificações, comentários podem tornar-se mentirosos.

A única verdade se encontra no código.

## Comentários não arrumam um código ruim

Muitas vezes, após escrever uma função que sabemos que está confusa nós pensamos: "É melhor eu comentar isso..."

Não! É melhor você limpar isso! Ao invés de gastar tempo comentando, gaste tempo refatorando.

## Explique-se no código

Ao invés de comentar uma condição, basta apenas criar uma função.

```java
// ruim
// Check to see if the employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65))

// bom
if (employee.isEligibleForFullBenefits())
```

## Bons comentários

>O único tipo de bom comentário, é aquele que você encontrou uma maneira de não escrevê-lo.

Entretanto, alguns comentários são perfeitamente justificáveis e benéficos.

### Comentários legais

Caso seja necessário escrever comentários sobre copyright, etc; tente se referir a algum arquivo externo ao invés de fazer um comentário imenso no código.

### Comentários Informativos

Comentários que são justificáveis para informação.

```java
// format matched kk:mm:ss EEE, MMM dd, yyyy
Pattern timeMatcher = Pattern.compile(“\\d*:\\d*:\\d* \\w*, \\w* \\d*, \\d*”);
```

Entretanto, ficaria melhor caso fosse criado uma classe auto-descritiva para obter essa informação, fazendo o comentário não ser tão necessário assim.

### Explicações sobre a intenção

Muitas vezes nos deparamos com comentários explicativos sobre algum tipo de solução e suas intenções por trás.

Esse tipo de comentário ao é o ideal, mas é válido para entendermos o ponto de vista do programador, e o que ele está tentando fazer com aquele pedaço de código escrito.

```java
public void testConcurrentAddWidgets() throws Exception {
    WidgetBuilder widgetBuilder = new WidgetBuilder(new Class[]{BoldWidget.class});
    String text = "'''bold text'''";
    ParentWidget parent = new BoldWidget(new MockWidgetRoot(), "'''bold text'''");
    AtomicBoolean failFlag = new AtomicBoolean();
    failFlag.set(false);

    //This is our best attempt to get a race condition
    //by creating large number of threads.
    for (int i = 0; i < 25000; i++) {
        WidgetBuilderThread widgetBuilderThread = new WidgetBuilderThread(widgetBuilder, text, parent, failFlag);
        Thread thread = new Thread(widgetBuilderThread);
        thread.start();
    }
    assertEquals(false, failFlag.get());
}
```

### Esclarecimentos

```java
public void testCompareTo() throws Exception {
    WikiPagePath a = PathParser.parse("PageA");
    WikiPagePath ab = PathParser.parse("PageA.PageB");
    WikiPagePath b = PathParser.parse("PageB");
    WikiPagePath aa = PathParser.parse("PageA.PageA");
    WikiPagePath bb = PathParser.parse("PageB.PageB");
    WikiPagePath ba = PathParser.parse("PageB.PageA");

    assertTrue(a.compareTo(a) == 0);    // a == a
    assertTrue(a.compareTo(b) != 0);    // a != b
    assertTrue(ab.compareTo(ab) == 0);  // ab == ab
    assertTrue(a.compareTo(b) == -1);   // a < b
    assertTrue(aa.compareTo(ab) == -1); // aa < ab
    assertTrue(ba.compareTo(bb) == -1); // ba < bb
    assertTrue(b.compareTo(a) == 1);    // b > a
    assertTrue(ab.compareTo(aa) == 1);  // ab > aa
    assertTrue(bb.compareTo(ba) == 1);  // bb > ba
}
```
Este tipo de comentário é arriscado, pois, o esclarecimento pode estar incorreto.
Portanto é importante verificar se não há outra forma, e caso não haja, prestar muita atenção para não repassar informação errada adiante.

### Avisos de consequências

Apesar de provavelmente existirem outra forma de lidar com essas consequências, comentários deste tipo são justificáveis

```java
public static SimpleDateFormat makeStandardHttpDateFormat() {
    //SimpleDateFormat is not thread safe,
    //so we need to create each instance independently.
    SimpleDateFormat df = new SimpleDateFormat("EEE, dd MMM yyyy HH:mm:ss z");
    df.setTimeZone(TimeZone.getTimeZone(”GMT”));
    return df;
}

```

### Comentários TODO

Comentários do tipo `//TODO` são justificáveis.

O que não é justificável é deixá-lo sujando o código para sempre.

Portanto, de tempos em tempos, escaneie seu projeto e transforme esses `//TODO` em `DONE`.

### Amplificação

Comentários que reafirmam a importância de um trecho de código que, olhando de fora, parece ser inconsequente.

```java
String listItemContent = match.group(3).trim();
// the trim is real important.  It removes the starting
// spaces that could cause the item to be recognized
// as another list.
new ListItemWidget(this, listItemContent, this.level + 1);
return buildList(text.substring(match.end()));
```

## Comentários Ruins

A maioria dos comentários que vemos por aí estão nessa categoria.

### Balbucia

```java
public void loadProperties() {
    try {
        String propertiesPath = propertiesLocation + "/" + PROPERTIES_FILE;
        FileInputStream propertiesStream = new
        FileInputStream(propertiesPath);
        loadedProperties.load(propertiesStream);
    }
    catch(IOException e) {
        // No properties files means all defaults are loaded
    }
}
```

O que o autor quis dizer com esse comentário? Não é muito conclusivo, podemos interpretar que, caso caia na exceção, então os defaults já estão carregados. Mas quando é executado o carregamento dessas propriedades? Será que foi carregado antes, depois, ou durante a execução da função? Será que o autor apenas não queria deixar o catch em branco? Ou pior, será que esse comentário é uma forma do autor lembrar que deve criar uma função no catch que faça o load das propriedades default?

A única solução é examinar o código, e, se temos que ler um comentário e então examinar o código para entendê-lo, o comentário é ruim.

### Comentários Redundantes

```java
// Utility method that returns when this.closed is true.
// Throws an exception if the timeout is reached.
public synchronized void waitForClose(final long timeoutMillis) throws Exception {
    if(!closed)
    {
        wait(timeoutMillis);
        if(!closed)
        throw new Exception("MockResponseSender could not be closed");
    }
}
```
Comentários redundantes na explicação de uma função.

```java

/**
* The processor delay for this component.
*/
protected int backgroundProcessorDelay = -1;

/**
* The lifecycle event support for this component.
*/
protected LifecycleSupport lifecycle = 
new LifecycleSupport(this);
```
Comentários redundantes na inicialização de variáveis.

### Comentários enganadores

```java
// Utility method that returns when this.closed is true.
// Throws an exception if the timeout is reached.
public synchronized void waitForClose(final long timeoutMillis) throws Exception {
    if(!closed)
    {
        wait(timeoutMillis);
        if(!closed)
        throw new Exception("MockResponseSender could not be closed");
    }
}
```
Este comentário, além de redundante, pode acabar confundindo o programador que ler e pensar que ele retorna `when` this.closed é `true`, sendo que na verdade ele retorna `if` this.closed é `true` após um timeout.

Ou seja, esse comentário definitivamente não deveria existir, transformou um código autoexplicativo em um possível problema.

### Comentários Obrigatórios

```java
/**
* 
* @param title The title of the CD
* @param author The author of the CD
* @param tracks The number of tracks on the CD
* @param durationInMinutes The duration of the CD in minutes
*/
public void addCD(String title, String author, int tracks, int durationInMinutes) {
    CD cd = new CD();
    cd.title = title;
    cd.author = author;
    cd.tracks = tracks;
    cd.duration = duration;
    cdList.add(cd);
}
```

Desnecessário ter uma regra de adicionar javadoc em toda função ou um comentário para cada variável.

Muitas vezes não agrega em nada e apenas serve para confundir o leitor e potencialmente propagar mentiras.

### Comentários Históricos

```java
/**
* Changes (from 11-Oct-2001)
* --------------------------
* 11-Oct-2001 : Re-organised the class and moved it to new package 
*               com.jrefinery.date (DG);
* 05-Nov-2001 : Added a getDescription() method, and 
eliminated NotableDate 
*               class (DG);
* 12-Nov-2001 : IBD requires setDescription() method, now that NotableDate 
*               class is gone (DG);  Changed getPreviousDayOfWeek(), 
*               getFollowingDayOfWeek() and getNearestDayOfWeek() to correct 
*               bugs (DG);
* 05-Dec-2001 : Fixed bug in SpreadsheetDate class (DG);
* 29-May-2002 : Moved the month constants into a separate interface 
*               (MonthConstants) (DG);
* 27-Aug-2002 : Fixed bug in addMonths() method, thanks to N???levka Petr (DG);
* 03-Oct-2002 : Fixed errors reported by Checkstyle (DG);
* 13-Mar-2003 : Implemented Serializable (DG);
* 29-May-2003 : Fixed bug in addMonths method (DG);
* 04-Sep-2003 : Implemented Comparable.  Updated the isInRange javadocs (DG);
* 05-Jan-2005 : Fixed bug in addYears() method (1096282) (DG);
**/
```

Cenas como essa eram comuns antigamente, quando não haviam bons softwares de source control e tudo mais.

Hoje em dia são **abomináveis**. Por favor, use o Git.

### Comentários sujeira

```java
/**
* Returns the day of the month.
*
* @return the day of the month.
*/
public int getDayOfMonth() {
    return dayOfMonth;
}
```

Não há motivos para fazer isso.

### Não use comentários quando se pode usar uma função

```java
// does the module from the global list <mod> depend on the
// subsystem we are part of?
if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem()))
```
Ruim.
```java
ArrayList moduleDependees = smodule.getDependSubsystems();
String ourSubSystem = subSysMod.getSubSystem();
if (moduleDependees.contains(ourSubSystem))
```
Bom.

### Posição de Marcação

As vezes parece fazer sentido marcarmos uma posição com um comentário do tipo:  
`// Actions ///////////////////////////`  
Entretanto, temos que tomar cuidado para usarmos poucas vezes, para não transformarmos a marcações em sujeira.

### Fechamento de Chaves

```java
public class wc {
    public static void main(String[] args) {
    BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
    String line;
    int lineCount = 0;
    int charCount = 0;
    int wordCount = 0;
    try {
        while ((line = in.readLine()) != null) {
        lineCount++;
        charCount += line.length();
        String words[] = line.split("\\W");
        wordCount += words.length;
        } //while
        System.out.println("wordCount = " + wordCount);
        System.out.println("lineCount = " + lineCount);
        System.out.println("charCount = " + charCount);
    } // try
    catch (IOException e) {
        System.err.println("Error:" + e.getMessage());
    } //catch
    } //main
}
```
Talvez seja útil para funções gigantes e com vários fechamentos de chaves. Entretanto, um código limpo possui funções pequenas.

### Código Comentado

```java
InputStreamResponse response = new InputStreamResponse();
    response.setBody(formatter.getResultStream(), formatter.getByteCount());
// InputStream resultsStream = formatter.getResultStream();
// StreamReader reader = new StreamReader(resultsStream);
// response.setContent(reader.read(formatter.getByteCount()));
```

O principal problema em se fazer isso é que o código ficará sujo para sempre, o próximo desenvolvedor que pegar esse trecho de código não apagará essa sujeira por precaução, ele não sabe se é importante para alguém. E nunca é.

Portanto, não faça isso, e, caso encontre algum código assim, apague-o. Hoje em dia temos softwares de source control que se lembrarão do código anterior para nós.

### Informação externa

Se você precisa escrever um comentário, escreva sobre o seu código local, não adicione contexto externo.

```java
/**
* Port on which fitnesse would run. Defaults to 8082.
*
* @param fitnessePort
*/
public void setFitnessePort(int fitnessePort)
{
    this.fitnessePort = fitnessePort;
}
```
Não sabemos se alguém irá editar esse comentário quando a porta não for mais a `8082`.

### Muita informação

Não coloque discussões históricas sobre a função que você está fazendo.

```java
/*
RFC 2045 - Multipurpose Internet Mail Extensions (MIME) 
Part One: Format of Internet Message Bodies
section 6.8.  Base64 Content-Transfer-Encoding
The encoding process represents 24-bit groups of input bits as output 
strings of 4 encoded characters. Proceeding from left to right, a 
24-bit input group is formed by concatenating 3 8-bit input groups. 
These 24 bits are then treated as 4 concatenated 6-bit groups, each 
of which is translated into a single digit in the base64 alphabet. 
When encoding a bit stream via the base64 encoding, the bit stream 
must be presumed to be ordered with the most-significant-bit first. 
That is, the first bit in the stream will be the high-order bit in 
the first 8-bit byte, and the eighth bit will be the low-order bit in 
the first 8-bit byte, and so on.
*/
```

## Exemplo

Segue exemplo tirado do livro de refatoração de código ruim com má utilização de comentários.

Antes: [GeneratePrimes.java][1]  
Depois: [PrimeGenerator.java][2]

[1]: ./exemplos/comentarios/GeneratePrimes.java
[2]: ./exemplos/comentarios/PrimesGenerator.java