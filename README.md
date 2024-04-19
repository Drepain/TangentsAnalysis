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
  ### `GameState.cs`
  Semelhante à classe `Entity.cs`, esta é uma classe abstrata que serve de base para o resto dos "Game States". As propriedades desta classe incluem:
   - O tamanho/resolução da janela (`width` e `height`)
   - uma interação com o GameStateManager (`gameStateManager`)

  ### `GameOverState`
  Esta classe é herdeira da classe `GameState.cs`, ela é ativada quando o player vai para fora da tela, causando um Game Over. Esta 'state' cria uma tela onde são denotadas as seguintes características:
   - uma opção para recomeçar o jogo (`playAgain`)
   - uma opção para voltar para o menu (`returnTitle`)
   - o score obtido (`score`)
   - o score máximo obtido (`hiScore`)

  ### `TitleState.cs`
  Esta classe é a primeira apresentada no jogo, podendo tambem ser acessada pela tela de Game Over. Esta classe disponibiliza na tela:
   - o titulo do jogo (`title`)
   - uma opção para começar o jogo (`start`)
   - uma intrução dando a entender que tecla pressionar para jogar (`instruction`)
   - os creditos do jogo (`credit`)
  
  ### `InGameState.cs`
  A classe principal, diria-mos, do jogo. Ao contrário dos outros game states, este é responsável por algumas features de gameplay. Algumas das propriedades incluem:
   - o objeto do player (`player`)
   - os objetos dos circulos em jogo (o array `circles`)
   - os limites de onde os circulos podem aparecer (`circleUpperBound` e `circleLowerBound`)
   - o score (`Score` e `scoreString` para representar na tela)

  Ao entrar neste estado, a classe irá realizar as seguintes tarefas:
   - reiniciar o score
   - criar o primeiro circulo, onde o player o estará a orbitar
   - calcular os limites de onde os novos circulos poderão ser criados, de acordo com o tamanho da janela
  
  No decorrer do jogo, este estado irá verificar:
   - se um botão do mouse foi clicado (ou se o ecrã foi tocado, no caso de mobile)
   - se o player está na distância necessária para orbitar um circulo
   - se o player saiu da tela
