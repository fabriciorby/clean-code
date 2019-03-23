# Funções

## Funções devem ser pequenas
Cada função deve ser pequena e *contar uma história*. Funções com `while`, `if` e `else` devem procurar possuir apenas uma linha, que provavelmente é uma chamada de função. Com isso, além da função ser pequena, é possível documentar o que ela está fazendo graças a um bom **nome significativo** e **descritivo**. A função também deve ter apenas um nível de abstração e fazer apenas **uma coisa**.

## Nomes descritivos
> Você sabe que está trabalhando com código limpo quando cada função é exatamente o que você esperasse que fosse.  
> Metade da batalha em criar um código limpo é idealizar esses nomes para funções, entretanto, quanto menor e mais focada a função for, é mais fácil de encontrar esse nome.

### Não tenha medo de escolher um nome grande.
É melhor uma função com um nome grande e explicativo do que:
- Uma função com nome pequeno e enigmático.
- Um comentário grande e explicativo sobre o que uma função faz.

## Parâmetros

> O número ideal de parâmetros a serem passados numa função é 0 (relação nulária).  
> Então temos 1 (relação unária), e 2 (relação binária).  
> Funções com 3 parâmetros (relação ternária) devem ser evitados quando possível.  
> Mais do que isso (relações n-árias), necessitam de uma justificativa muito especial - e mesmo assim não deveriam ser usadas.  

Uma função com muitos parâmetros é uma função difícil de ler. Além disso, é difícil de testar. Devemos escrever todos os casos de teste para todas as combinações possíveis de parâmetro funcionarem apropriadamente.

Criar uma função `void` para mudar algum objeto por "referência" passando por parâmetro é um problema também, normalmente esperamos que a função tenha uma entrada e acarrete em uma saída através do valor de retorno.

## Flags como parâmetro

Não é uma boa prática.

Se uma função precisa de uma flag para saber qual decisão deve tomar, é porque ela pode ser dividida em duas funções menores.

## Funções Unárias

Há dois casos mais comuns para a utilização desse tipo de função. Você pode estar perguntando algo para a função sobre o parâmetro, como em `boolean fileExists(String filePath)`. E pode também passar um parâmetro que será modificado e então retornado, como em `InputStream fileOpen(String filePath)`.

Entretanto, há também a forma de criar a função em forma de evento, ou seja, uma função `void`. Neste caso, deve ser utilizada com cautela e de forma bem clara a informar ao leitor de que é um evento.

## Funções Binárias

Funções binárias são mais confusas e é bom evitar. Se for possível convertê-las em unárias, é encorajado que se faça isso.

Funções como `new Point(0,0)` não causam problemas, visto que são 2 componentes ordenados que compõem um valor. Entretanto, na maioria das vezes, um dos parâmetros é um mero coadjuvante que pode ser ignorado durante a leitura. E ao ignorá-lo na leitura, pode-se deixar passar algum erro.

## Funções Ternárias

Funções ternárias são ainda mais confusas.

Por exemplo, `AssertEquals(message, expected, actual)`, apesar de ser uma função que de certa forma é uma convenção, muitas vezes temos que ler mais de uma vez para descobrimos que temos que ignorar o campo `message`, e nos atentar para não confundir a ordem dos dois parâmetros seguintes.

## Objetificando o parâmetro

Quando uma função parece que necessita de mais de 2 ou 3 parâmetros, provavelmente alguns desses parâmetros podem ser utilizados em uma nova classe.

```java
//ruim
Circle makeCircle (double x, double y, double radius);

//bom
Circle makeCircle (Point center, double radius);
```

## `varargs`

As vezes precisamos passar uma lista de parâmetros na função.

```java
//assinatura
public String format(String format, Object... args);

//utilização
String.format("Meu nome é %s e eu tenho %s anos.", nome, idade);
```
Em casos como o de cima, onde as variáveis são tratadas da mesma forma, elas são equivalentes a uma variável do tipo `List` e, portanto, a função acima é binária.

```java
void monad(Integer… args);
void dyad(String name, Integer… args);
void triad(String name, int count, Integer… args);
```

Mais sobre `varargs` em Java: http://jtechies.blogspot.com/2012/07/item-42-use-varargs-judiciously.html

## Verbos e palavras-chave

Utilize verbos + substantivos para evocar uma ação, procure utilizar palavras-chave para facilitar a leitura e compreensão.

```java
//bom
write(name);

//melhor
writeField(name);
```
A palavra-chave descreve a ação, e facilita a leitura e entendimento.

```java
//bom
assertEquals(expected, actual);

//melhor
assertExpectedEqualsActual(expected, actual);
```
O problema de lembrar a ordem dos argumentos foi mitigado.

## Não ter efeitos colaterais

Uma função deve fazer apenas uma coisa.

Ao chamarmos uma função, ela deve executar apenas o papel definido por seu escopo.  
Caso ela faça algo a mais, aumentamos a dificuldade de leitura do código.

## Parâmetros de Saída

Parâmetros são naturalmente interpretados como um valor de entrada de uma função, ao chamarmos uma função que utilizará esse parâmetro apenas para modificá-lo, mas não retorná-lo, aumentamos a dificuldade de leitura do código.

```java
//ruim
appendFooter(s);

//o que é 's'?
public void appendFooter(StringBuffer report)
```
Uma boa solução pode ser internalizar essa função dentro do próprio objeto.

```java
//bom
report.appendFooter();
```

## Separe as responsabilidades

Funções devem fazer algo ou responder algo, mas não os dois ao mesmo tempo, pois, fazendo assim, aumentamos a dificuldade de leitura do código.

Por exemplo, a função ```public boolean set(String attribute, String value)``` atribui um valor a um atributo e, caso obtenha sucesso, retorna ```true```, caso contrário ```false```.

```java
//ruim
if (set("username", "unclebob"))...
```

O leitor pode e vai se confundir, pensando que estamos checando se ```unclebob``` existe no atributo ```username```.

```java
//bom
if (attributeExists("username")) {
    setAttribute("username", "unclebob");
    ...
}
```

## Prefira estourar exceptions a retornar códigos de erro

Retornar código de erro na função faz com que você precise lidar com o erro imediatamente.  
Entretanto, caso seja utilizado a Exception, você pode simplesmente tratar o erro no catch, separando o erro da parte feliz do código.

## Extraia o Try/Catch

Ao separar o try/catch de forma com que cada função tenha apenas uma responsabilidade, o código fica mais fácil de entender e de ser modificado.

```java
public void delete(Page page) {
    try {
        deletePageAndAllReferences(page);
    } catch (Exception e) {
        logError(e);
    }
}
```
Tratar o erro é uma responsabilidade, e é isso que a função `delete` faz.

```java
private void deletePageAndAllReferences(Page page) throws Exception {
    deletePage(page);
    registry.deleteReference(page.name);
    configKeys.deleteKey(page.name.makeKey());
}
```
A função `deletePageAndAllReferences` lida apenas com o processo de deletar a página.
Dessa forma, nessa função, todo o papel de lidar com erros pode ser ignorado.

```java
private void logError(Exception e) {
    logger.log(e.getMessage());
}
```
A função `logError` lida apenas com a parte de logar o erro.

## Não se repita

Duplicações são a semente de todo o mal em um software. A POO foi criada para utilizarmos classes, heranças, etc exatamente para eliminarmos a redundância de nosso código.

## Como escrever funções assim?

Escrever software é como escrever qualquer outra coisa. Fazemos um rascunho e então o melhoramos gradualmente.  

Inicialmente escreveremos funções todas tortas, desorganizadas e de difícil entendimento.  
Entretanto, após criarmos os testes unitários completos e sabermos exatamente como o software deve se comportar, podemos refatorar nosso código.

Pense que as funções devem contar histórias para seus leitores, e tente contar essas histórias da melhor maneira possível.
