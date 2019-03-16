# Nomes Significativos

Porque sempre estamos dando nomes a tudo em nosso código, é importante fazê-lo bem.

Parar para pensar em um bom nome leva um certo tempo, mas **economiza** mais tempo do que levamos para fazê-lo.

Em funções, classes e variáveis; se um nome necessita de um comentário, então ele não é um bom nome, e você deveria pensar em outro.

```Java
//código sujo
public static void copyChars(char a1[], char a2[]) {
    for (int i = 0; i < a1.length; i++) {
        a2[i] = a1[i];
    }
}

//código limpo
public static void copyChars(char origem[], char destino[]) {
    for (int i = 0; i < source.length; i++) {
        destino[i] = origem[i];
    }
}
```
Pense em nomes **informativos**, **descritivos**, **pesquisáveis** e **pronunciáveis**. 

## Não utilize prefixos ou notação húngara

 ````Java
 //código sujo
 public class Part {
    private String m_dsc; //não utilize prefixos e/ou abreviações!
    void setName(String name) {
        m_dsc = name;
    }
}

//código sujo
public class Part {
    String descriptionString; //não utilize notação húngara!
    void setDescription(String description) {
        this.descriptionString = description;
    }
}

//código limpo
public class Part {
    String description;
    void setDescription(String description) {
        this.description = description;
    }
}
````

## Não utilize mapas mentais

> O pior motivo para se nomear uma variável com nome **c** é porque **a** e **b** já foram utilizados.

Normalmente o desenvolvedor é uma pessoa esperta.

Se o homem está desenvolvendo e se lembra de cabeça que a variável **r** significa a reposta em lower-case da classe **tal** a partir da url **x**; então ele deve ser muito esperto *mesmo*.

Entretanto, a diferença entre um desenvolvedor esperto e um profissional é que o profissional entende que a clareza é o fator mais importante em um código. E, portanto, ele utiliza de sua esperteza para aumentar a clareza do código para outros desenvolvedores.

## Classes

Nomes de classes devem ser **substantivos**.

## Métodos

Nomes de métodos devem ser **verbos**.

Utilize a convenção de acessores, mutadores e predicados com prefixos **get**, **set** e **is**.

````Java
    String nome = cliente.getNome();

    cliente.setName("Yamamoto");

    if (cliente.isPessoaFisica())
````

Dar preferência para utilização de métodos estáticos descritivos ao invés de construtores.

````Java
//ok
Complex fulcrumPoint = new Complex(23.0);

//melhor
Complex fulcrumPoint = Complex.FromRealNumber(23.0);
````

## Não seja *fofo*

Não invente, não utilize nomes engraçados, seja direto e conciso.

## Utilize uma palavra por conceito, mas seja coerente

Procure sempre utilizar a mesma expressão para diversas operações semelhantes em seus métodos.

Se seu método adiciona algum valor em uma lista e você chama este método com a expressão `add`, não chame outro método que faz uma ação semelhante com a expressão `adiciona`.

Entretanto, tenha cuidado para não perder a coerência. Se temos um método que, ao ser chamado, adiciona valores concatenando uma lista em outra. Neste caso, talvez devêssemos utilizar outra expressão, tal qual `append`.

## Utilize Nomes Técnicos

Quando necessário, utilize nomes técnicos.

Nomes de algoritmos, funções matemáticas, patterns, etc...

A pessoa que for ler o código será um desenvolvedor, e será mais fácil de entender se o nome lhe trouxer um conceito.

O desenvolvedor que se deparar com uma classe chamada *CarrinhoStrategy*, saberá que se trata da implementação da pattern *Strategy*.

## Adicione Contextos Significativos

Sobre uma variável chamada `estado` sendo utilizada sozinha em uma classe, é possível saber do que ela se trata?

Podemos adicionar um contexto ao nome da variável, por exemplo `enderecoEstado`. Sabemos agora que se trata de um estado de um endereço.

É preferível que se utilize uma classe `Endereco`, neste caso até o compilador saberia do que se trata.

É importante saber que, além da nomenclatura da variável, o contexto pode ser adquirido através da nomenclatura dos métodos.

## Palavras Finais

Escolher nomes bons é uma tarefa difícil. Entretanto, ao se deparar com código com nomes que poderiam ser melhor, não hesite em renomear (para melhor). As pessoas serão gratas por isso, no curto e, principalmente, no longo prazo.