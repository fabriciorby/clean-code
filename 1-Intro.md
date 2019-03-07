# Introdução

> Writing clean code is what you must do in order to call yourself a professional. There is no reasonable excuse for doing anything less than your best.

Clean Code - *A Handbook of Agile Software Craftsmanship* é um livro escrito por Robert C. Martin (Uncle Bob), que é famoso por ter participado da concepção do Agile Manifesto.

Sua premissa é de que, ao terminarmos de lê-lo, saibamos diferenciar o código bom do código ruim. Saibamos escrever um bom código e transformar um código ruim em um bom.

O livro segue, primeiramente, por **abominar** os códigos que não possuem fácil leitura, argumentando e apontando diversas razões pelo qual cada linha de código dos exemplos citados são uma **bagunça**; demonstrando contra exemplos úteis para que possamos entender como uma simples refatoração pode nos fazer economizar tempo (dinheiro) na interpretação de um código.

## Código Ruim

> É claro que um código ruim já lhe atrasou. Mas, então, por que você o escreveu dessa forma?

Muitas vezes por conta da pressa, prazos, falta de vontade, decidimos que queremos finalizar o trecho de código rapidamente. Fazer *deploy* de um código apenas porque ele está funcionando não é uma boa prática. É de suma importância executar um bom código e, não deixar para refatorá-lo no dia seguinte; todos sabemos que **o dia seguinte nunca chega**.

O custo de um código confuso aumenta exponencialmente em uma corporação, a manutenção de algo fácil passa a ser uma tarefa árdua e **infinita**. Várias vezes nos deparamos com uma necessidade de alteração num pedaço de código que "*é só trocar um if*" e no fim demoramos aproximadamente 2 semanas para resolvê-lo.

Sabe o que é o pior de tudo? A culpa é **nossa**.

Enquanto *negócios* busca cumprir metas e prazos apertados, não devemos corromper nosso código em troca de uma entrega.

> Eles podem proteger com paixão os prazos e requisitos, mas essa é a função deles. A *sua* é proteger o código com a mesma paixão.

Ceder aos prazos em troca de entrega de código ruim **não é uma atitude profissional**.

Se bagunças antigas reduzem o rendimento, e sempre nos são impostos prazos cada vez mais apertados. A única maneira de ir mais rápido e não perder o prazo é: **sempre manter o código limpo**. 

Afinal, código ruim hoje é manutenção infinita amanhã.

## Código Limpo

> Escrever um código limpo é como pintar um quadro. A maioria de nós sabe quando a figura foi bem ou mal pintada; mas isso não quer saber que você saiba pintar.

De todas as definições de *Código Limpo*, a mais resumida e que mais me chamou atenção foi: **código limpo é o código escrito por quem se importa**.

De fato, o código limpo deve ser idealizado, de forma que seja de fácil interpretação e manutenção; deve ter testes unitários e não possuir duplicações. É um código que você lê e não vê nenhum ponto de melhoria, pois **o autor já se preocupou com isso**.

> Um código limpo é um código que foi cuidado por alguém. Alguém que calmamente o manteve simples e organizado; alguém que prestou atenção necessária aos detalhes; **alguém que se importou**. - Michael Feathers