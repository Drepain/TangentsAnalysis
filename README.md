# TangentsAnalysis

Uma análise simples ao projeto "[Tangents](https://github.com/danestrin/Tangents/)" desenvolvido por [danestrin](https://github.com/danestrin/).
Realizado no âmbito da UC TDV.

## Membros do grupo

Francisco Gomes - 27941
Jorge Costa - 27923

## Descrição do Jogo

Tangents é um jogo 2D baseado em geometria, usando como base C# e [MonoGame](https://monogame.net). 

Neste jogo o player controla um pequeno circulo azul e começa o jogo em volta de um ciculo maior e vermelho. Interagindo com o rato, o jogador é lançado em linha reta sobre a linha tangente do circulo em que se encontrava com o objetivo de passar ao próximo circulo antes que a tela o ultrapasse, concluindo este objetivo é adicionado um ponto.

## Plataformas

Tangents é um jogo multiplataforma, sendo suportado em todos os principais sistemas operativos (PC, Android, iOS), obtendo melhor desempenho em Android.

# Estrutura e Organização do código

As classes principais deste projeto estão divididas em 3 categorias:
 - **GameObjects** - Contem as classes mais relevantes aos objetos principais do jogo e da gameplay em si (o player, os circulos, etc.);
 - **GameState** - Contem as classes que controlam o estado do jogo (menus e interações que podem levar a menus)
 - **Managers** - Estas classes servem para, como o nome indica, gerenciar o jogo, no sentido de controlar os aspetos principais da aplicação em si (input, score, etc.)

 ## GameObjects
 ### `Entity.cs`
  Uma classe abstrata que serve para generalizar alguns metodos relevantes às entidades principais do jogo, sendo elas o Circle e o Player. Ela define propriedades como:
   - o sprite utilizado (`image`)
   - as coordenadas da posição do objeto (`x` e `y`)
   - o raio do sprite (`Radius`)[^1]
   - a expressura da linha do objeto (`Thickness`)[^1]
   - a posição do objeto (`Position`)

 [^1]: Como as classes herdeiras controlam sprites de circunferencias, faz sentido ter uma propriedade deste tipo.

 ### `Circle.cs`