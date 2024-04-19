# TangentsAnalysis

Uma análise simples ao projeto "[Tangents](https://github.com/danestrin/Tangents/)" desenvolvido por [danestrin](https://github.com/danestrin/).
Realizado no âmbito da UC TDV.

## Membros do grupo

Francisco Gomes - 27941;
Jorge Costa - 27923;

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
  Uma classe herdada de `Entity.cs` que controla a maior parte dos aspetos dos circulos que o jogador tem que orbitar. Esta classe tem todas as propriedades da sua classe abstrata, assim como:
   - a velocidade a que atravessa a tela (`speed`)
   - uma variavel para verificar se o player está a orbitar o circulo (`PlayerAttached`)

> [!NOTE]
> `PlayerAttached` é do tipo [event](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/events/). Pensamos que seria possível substituir esta implementação por uma do tipo [bool](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/bool), sendo que a maioria dos métodos em que esta variavel é usada podia, também, um bool ser usado num contexto semelhante.

  Esta classe também apresenta os seus próprios métodos com relevância:
   - `OnPlayerAttached`: verifica se o player está a orbitar o circulo;
   - `HandleCollision`: quando o player se lança para o próximo circulo, este método verifica se o player está na distância de colisão do circulo, aumentando, ou não, o score devidamente;

### `Player.cs`
  Uma classe herdada de `Entity.cs` que controla a maior parte dos aspetos do circulo controlado pelo jogador, aprofundando em propriedades relevantes para tal.
   - o angulo e a velocidade angular (`angle` e `angularVelocity`)
   - a velocidade do player quando transitando de um circulo para o outro (`speed` e `speedConstant`)
   - uma variavel que indica se o player está conectado a um circulo (`IsOrbiting`)
   - o circulo (se estiver conectado a um) a que o player está conectado (`AttachedCircle`)
   - a tangente do ponto onde o player está quando atirado e a sua trajetoria (`trajectory` e `TangentPoint`)

  Na função `Update`, que corre a cada frame, é verificado se o player está a orbitar um circulo. Se estiver, ele continua a orbitar. Se ele não estiver, ele é lançado mais longe na sua trajetoria.

  ### `GameText.cs`
  A classe que é responsável por métodos e propriedades de texto. Estas não têm muito de interesse e são padrão para a maioria de aplicações (centrar texto, fonte, etc.)

  > [!NOTE]
  > Faria bastante sentido esta classe fazer parte da categoria **Managers**, sendo que a classe não faz muito por si, mas gerência uma parte mais geral da aplicação em si.

  ## GameState