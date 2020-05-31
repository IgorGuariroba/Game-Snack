# Game-Snack
Neste tópico Com o auxilio de um repositório no github criei este jogo da cobrinha    
Vamos começar criando um arquivo “snake.html” que conterá todo o nosso código.

Como este é um arquivo HTML, a primeira coisa que precisamos é da <!DOCTYdeclaração PE> . In snake.html e digite o seguinte:

Ótimo, agora vá em frente e abra snake.htmlno seu navegador preferido. Você deve poder ver Welcome to Snake!


snake.html aberto no chrome
Estamos começando bem?

Criando a tela
Para poder criar nosso jogo, precisamos usar o HTML <canvcomo>. É isso que é usado para desenhar gráficos usando JavaScript.

Substitua a mensagem de boas-vindas snake.htmlpelo seguinte:

<canvas id="gameCanvas" width="300" height="300">&lt;canvas>
O ID é o que identifica a tela e deve sempre ser especificado. Vamos usá-lo para acessar a tela mais tarde. A largura e a altura são as dimensões da tela e também devem ser especificadas. Nesse caso, 300 x 300 pixels.

Seu arquivo snake.html agora deve ficar assim.

Se você atualizar a página do navegador onde você abriu anteriormente snake.html, verá uma página em branco. Isso ocorre porque, por padrão, a tela está vazia e não possui plano de fundo. Vamos consertar isso. ?

Dê à tela uma cor de fundo e uma borda
Para tornar nossa tela visível, podemos definir uma borda escrevendo algum código JavaScript. Para fazer isso, precisamos inserir <script><; / script> tag s after the </canvas>, para onde todo o nosso código JavaScript irá.

Se você colocar a <scritag pt> antes do e the &lcanvas>, seu código não funcionará, pois o HTML não será carregado.
Agora podemos escrever um código JavaScript entre as <script><tags / script> anexadas. Atualize seu código como abaixo.

Primeiro, obtemos o elemento canvas usando o ID (gameCanvas) especificado anteriormente. Em seguida, obtemos o contexto "2d" da tela, o que significa que estaremos desenhando no espaço 2D.

Finalmente, desenhamos um retângulo branco de 300 x 300 com uma borda preta. Isso cobre a tela inteira, começando no canto superior esquerdo (0, 0).

Se você recarregar snake.htmlno navegador, verá uma caixa branca com uma borda preta! Bom trabalho, temos uma tela que podemos usar para criar nosso jogo de cobra! ? Para o próximo desafio!

Representando nossa cobra
Para o nosso jogo de cobra funcionar, precisamos saber a localização da cobra na tela. Para fazer isso, podemos representar a cobra como uma matriz de coordenadas. Assim, para criar uma cobra horizontal no meio da tela (150, 150), podemos escrever o seguinte:

let snake = [  {x: 150, y: 150},  {x: 140, y: 150},  {x: 130, y: 150},  {x: 120, y: 150},  {x: 110, y: 150},];
Observe que a coordenada y para todas as partes é sempre 150. A coordenada x de cada parte é -10px (à esquerda) da parte anterior. O primeiro par de coordenadas na matriz {x: 150, y: 150}representa a cabeça à direita da cobra.

Isso ficará mais claro quando desenharmos a cobra na próxima seção.

Criando e desenhando nossa cobra
Para exibir a cobra na tela, podemos escrever uma função para desenhar um retângulo para cada par de coordenadas.

function drawSnakePart(snakePart) {  ctx.fillStyle = 'lightgreen';  ctx.strokestyle = 'darkgreen';
  ctx.fillRect(snakePart.x, snakePart.y, 10, 10);  ctx.strokeRect(snakePart.x, snakePart.y, 10, 10);}
Em seguida, podemos criar outra função que imprime as partes na tela.

function drawSnake() {  snake.forEach(drawSnakePart);}
Nosso snake.htmlarquivo agora deve ficar assim:

Se você atualizar a página do navegador agora, verá uma cobra verde no meio da tela. Impressionante! ?


Permitindo que a cobra se mova horizontalmente
Em seguida, queremos dar à cobra a capacidade de se mover. Mas como nós fazemos isso? ?

Bem, para fazer a cobra se mover um passo (10px) para a direita, podemos aumentar a coordenada x de cada parte da cobra em 10px (dx = + 10px). Para fazer a cobra se mover para a esquerda, podemos diminuir a coordenada x de cada parte da cobra em 10px (dx = -10).

dx é a velocidade horizontal da cobra.
A criação de uma cobra que moveu 10px para a direita deve ficar assim


Crie uma função chamada advanceSnakeque usaremos para atualizar a cobra.

function advanceSnake() {  const head = {x: snake[0].x + dx, y: snake[0].y};
  snake.unshift(head);
  snake.pop();}
Primeiro, criamos uma nova cabeça para a cobra. Em seguida, adicionamos a nova cabeça ao início da cobra usando unshift e removemos o último elemento da cobra usando pop . Dessa forma, todas as outras partes da cobra se encaixam no lugar, como mostrado acima.

Boom ?, você está entendendo isso.

Permitindo que a cobra se mova verticalmente
Para mover nossa cobra para cima e para baixo, não podemos alterar todas as coordenadas y em 10px. Isso mudaria a cobra inteira para cima e para baixo.


Em vez disso, podemos alterar a coordenada y da cabeça. Diminuindo em 10px para mover a cobra para baixo e aumentando em 10px para mover a cobra para cima. Isso fará a cobra se mover corretamente.

Felizmente, devido à maneira como escrevemos a advanceSnakefunção, isso é muito fácil de fazer. No interior advanceSnake, atualize a cabeça para também aumentar a coordenada y da cabeça em dy .

const head = {x: snake[0].x + dx, y: snake[0].y + dy};
Para testar como nossa advanceSnakefunção funciona, podemos chamá-lo temporariamente antes da drawSnakefunção.

// Move on step to the rightadvanceSnake()
// Change vertical velocity to 0dx = 0;// Change horizontal velocity to 10dy = -10;
// Move one step upadvanceSnake();
// Draw snake on the canvasdrawSnake();
É assim que nosso snake.htmlarquivo parece até agora.

Atualizando o navegador, podemos ver que nossa cobra se moveu. Sucesso!


Refatorando nosso código
Antes de prosseguirmos, vamos refatorar e mover o código que desenha a tela dentro de uma função. Isso nos ajudará na próxima seção.

“A refatoração de código é o processo de reestruturação do código de computador existente , sem alterar seu comportamento externo.” - wikipedia
function clearCanvas() {  ctx.fillStyle = "white";  ctx.strokeStyle = "black";
  ctx.fillRect(0, 0, gameCanvas.width, gameCanvas.height);  ctx.strokeRect(0, 0, gameCanvas.width, gameCanvas.height);}
Estamos fazendo grandes avanços! ?

Fazendo nossa cobra se mover automaticamente
Ok, agora que refatoramos nosso código com sucesso, podemos fazer nossa cobra se mover automaticamente.

Antes, para testar se nossa advanceSnakefunção funcionava, chamamos duas vezes. Uma vez para fazer a cobra se mover para a direita, e uma vez para fazer a cobra subir.

Assim, se quiséssemos fazer a cobra se mover cinco passos para a direita, chamaríamos advanceSnake()cinco vezes seguidas.

clearCanvas();advanceSnake();advanceSnake();advanceSnake();advanceSnake();advanceSnake();drawSnake();
Mas, chamá-lo cinco vezes seguidas, como mostrado acima, fará a cobra saltar 50px para frente.


Em vez disso, queremos fazer com que a cobra pareça estar avançando passo a passo.

Para fazer isso, podemos adicionar um pequeno atraso entre cada chamada, usando setTimeout . Também precisamos ter certeza de ligar drawSnakesempre que ligarmos advanceSnake. Se não o fizermos, não conseguiremos ver os passos intermediários que mostram a cobra se movendo.

setTimeout(function onTick() {  clearCanvas();  advanceSnake();  drawSnake();}, 100);
setTimeout(function onTick() {  clearCanvas();  advanceSnake();  drawSnake();}, 100);
...
drawSnake();
Observe como também chamamos clearCanvas()dentro de cada um setTimeout. Isso é para remover todas as posições anteriores da cobra que deixariam uma trilha para trás.


Embora haja um problema com o código acima. Não há nada aqui para informar ao programa que ele precisa aguardar setTimeout antes de passar para o próximo setTimeout . Isso significa que a cobra ainda vai pular 50px para frente, mas após um pequeno atraso .

Para consertar isso, precisamos envolver nosso código dentro de funções, chamando uma função de cada vez.

stepOne();    function stepOne() {  setTimeout(function onTick() {    clearCanvas();    advanceSnake();    drawSnake();   // Call the second function   stepTwo();  }, 100)}
function stepTwo() {  setTimeout(function onTick() {    clearCanvas();    advanceSnake();    drawSnake();    // Call the third function    stepThree();  }, 100)}
...
Como fazemos nossa cobra continuar se movendo? Em vez de criar um número infinito de funções que se chamam, podemos criar uma função maine chamá-la repetidamente.

function main() {  setTimeout(function onTick() {    clearCanvas();    advanceSnake();    drawSnake();
    // Call main again    main();  }, 100)}
Voilà! Agora temos uma cobra que continuará se movendo para a direita. Embora, uma vez que chegue ao fim da tela, continue sua jornada infinita pelo desconhecido? Vamos consertar isso no devido tempo, paciência jovem padawan. ?


Mudando a direção da cobra
Nossa próxima tarefa é mudar a direção da cobra quando uma das teclas de seta é pressionada. Adicione o seguinte código após a drawSnakePartfunção.

function changeDirection(event) {  const LEFT_KEY = 37;  const RIGHT_KEY = 39;  const UP_KEY = 38;  const DOWN_KEY = 40;
  const keyPressed = event.keyCode;  const goingUp = dy === -10;  const goingDown = dy === 10;  const goingRight = dx === 10;  const goingLeft = dx === -10;
  if (keyPressed === LEFT_KEY && !goingRight) {    dx = -10;    dy = 0;  }
  if (keyPressed === UP_KEY && !goingDown) {    dx = 0;    dy = -10;  }
  if (keyPressed === RIGHT_KEY && !goingLeft) {    dx = 10;    dy = 0;  }
  if (keyPressed === DOWN_KEY && !goingDown) {    dx = 0;    dy = 10;  }}
Não há nada complicado acontecendo aqui. Verificamos se a tecla pressionada corresponde a uma das teclas de seta. Nesse caso, alteramos a velocidade vertical e horizontal conforme descrito anteriormente.

Observe que também verificamos se a cobra está se movendo na direção oposta à nova direção pretendida. Isso evita que a cobra se inverta, por exemplo, quando você pressiona a tecla de seta para a direita quando a cobra está se movendo para a esquerda.


Inversão de cobra
Para se conectar changeDirectionao nosso jogo, podemos usar addEventListener no documento para 'ouvir' quando uma tecla é pressionada. Em seguida, podemos ligar changeDirectioncom o evento keydown . Adicione o seguinte código após a mainfunção.

document.addEventListener("keydown", changeDirection)
Agora você deve mudar a direção da cobra usando as quatro teclas de seta. Bom trabalho, você está pegando fogo ?!

A seguir, vamos ver como podemos gerar alimentos e cultivar nossa cobra.

Gerando comida para a cobra
Para o nosso alimento de cobra, temos que gerar um conjunto aleatório de coordenadas. Podemos usar uma função auxiliar randomTenpara produzir dois números. Um para a coordenada x e um para a coordenada y.

Também temos que garantir que a comida não esteja localizada onde a cobra está atualmente. Se for, temos que gerar um novo local de comida.

function randomTen(min, max) {  return Math.round((Math.random() * (max-min) + min) / 10) * 10;}
function createFood() {  foodX = randomTen(0, gameCanvas.width - 10);  foodY = randomTen(0, gameCanvas.height - 10);
  snake.forEach(function isFoodOnSnake(part) {    const foodIsOnSnake = part.x == foodX && part.y == foodY    if (foodIsOnSnake)      createFood();  });}
Temos então que criar uma função para desenhar a comida na tela.

function drawFood() { ctx.fillStyle = 'red'; ctx.strokestyle = 'darkred'; ctx.fillRect(foodX, foodY, 10, 10); ctx.strokeRect(foodX, foodY, 10, 10);}
Finalmente, podemos ligar createFoodantes de ligar main. Não se esqueça de atualizar também mainpara usar a drawFoodfunção.

function main() {  setTimeout(function onTick() {    clearCanvas();    drawFood()    advanceSnake();    drawSnake();
    main();  }, 100)}
Crescendo a cobra
Crescer nossa cobra é simples. Podemos atualizar nossa advanceSnakefunção para verificar se a cabeça da cobra está tocando a comida. Se for, podemos pular a remoção da última parte da cobra e criar um novo local de comida.

function advanceSnake() {  const head = {x: snake[0].x + dx, y: snake[0].y};
  snake.unshift(head);
  const didEatFood = snake[0].x === foodX && snake[0].y === foodY;  if (didEatFood) {    createFood();  } else {    snake.pop();  }}
Acompanhar a pontuação
Para tornar o jogo mais agradável para o jogador, também podemos adicionar uma pontuação que aumenta quando a cobra come comida.

Crie uma nova pontuação variável e defina-a como 0 após a declaração da cobra.

let score = 0;
Em seguida, adicione uma nova div com o ID "score" antes da tela. Podemos usar isso para exibir a pontuação.

<div id="score">0</div><canvas id="gameCanvas" width="300" height="300"></canvas>
Por fim, atualize advanceSnakepara aumentar e exibir a pontuação quando a cobra comer a comida.

function advanceSnake() {  ...
  if (didEatFood) {    score += 10;    document.getElementById('score').innerHTML = score;
    createFood();  } else {    ...  }}
Pheew, isso foi bastante, mas já percorremos um longo caminho?

Terminar o jogo
Ainda resta uma peça final, que é o fim do jogo. Para fazer isso, podemos criar uma função d idGameEnd que os retornos t rue quando o jogo terminou ou f ALSE contrário.

function didGameEnd() {  for (let i = 4; i < snake.length; i++) {    const didCollide = snake[i].x === snake[0].x &&      snake[i].y === snake[0].y
    if (didCollide) return true  }
  const hitLeftWall = snake[0].x < 0;  const hitRightWall = snake[0].x > gameCanvas.width - 10;  const hitToptWall = snake[0].y &lt; 0;  const hitBottomWall = snake[0].y > gameCanvas.height - 10;
  return hitLeftWall ||          hitRightWall ||          hitToptWall ||         hitBottomWall}
Primeiro, verificamos se a cabeça da cobra toca outra parte da cobra e retornamos verdadeira, se o fizer.

Observe que começamos nosso loop a partir do índice 4. Há duas razões para isso. A primeira é que didCollide avaliaria imediatamente se true se o índice fosse 0, então o jogo terminaria. A segunda é que é impossível que as três primeiras partes se toquem.
Em seguida, verificamos se a cobra atingiu alguma das paredes da tela e retornamos true, se não, caso contrário, retornamos false .

Agora podemos retornar no início de nossa mainfunção se didEndGameretornar verdadeiro, encerrando assim o jogo.

function main() {  if (didGameEnd()) return;
  ...}
Nosso snake.html agora deve ficar assim:

Agora você tem um jogo de cobra em funcionamento que pode jogar e compartilhar com seus amigos. Mas antes de comemorar, vamos analisar um problema final. Este será o último, eu prometo.

Erros sorrateiros?
Se você jogar o jogo várias vezes, poderá perceber que às vezes o jogo termina inesperadamente. Este é um exemplo muito bom de como os bugs podem se infiltrar em nossos programas e causar problemas?

Quando um erro ocorre, a melhor maneira de resolvê-lo é primeiro ter uma maneira confiável de reproduzi-lo. Ou seja, crie as etapas precisas que levam ao comportamento inesperado. Então você precisa entender por que eles causam um comportamento inesperado e, em seguida, apresentar uma solução.

Reproduzindo o bug
No nosso caso, as etapas executadas para reproduzir o bug são as seguintes:

A cobra está se movendo para a esquerda
O jogador pressiona a tecla de seta para baixo
O jogador pressiona imediatamente a tecla de seta para a direita (antes de decorridos 100ms)
O jogo termina

Compreendendo o erro
Vamos detalhar o que acontece passo a passo.

Cobra está se movendo para a esquerda

Velocidade horizontal, dx é igual a -10
main função é chamada
advanceSnake é chamado que avança a cobra -10px para a esquerda.
O jogador pressiona a tecla de seta para baixo

changeDirection é chamado
keyPressed === DOWN_KEY && dy !goingUp avalia como verdadeiro
dx muda para 0
dy muda para +10
O jogador pressiona imediatamente a seta direita (antes de decorridos 100ms)

changeDirection é chamado
keyPressed === RIGHT_KEY && !goingLeft avalia como verdadeiro
dx muda para +10
dy muda para 0
O jogo termina

mainA função é chamada após o término de 100ms.
advanceSnake é chamado que avança a cobra 10px para a direita.
const didCollide = snake[i].x === snake[0].x && snake[i].y === snake[0].y avalia como verdadeiro
didGameEnd retorna verdadeiro
main função retorna cedo
O jogo termina
Corrigindo o erro
Depois de estudar o que aconteceu, aprendemos que o jogo terminou porque a cobra se inverteu.

Isso ocorre porque quando o jogador pressiona a seta para baixo, dx é definido como 0. Assim, é keyPressed === RIGHT_KEY && !goingLeftavaliado como true e dx é alterado para 10.

É importante notar que a mudança de direção ocorreu antes do decurso de 100ms. Se 100ms decaíssem, a cobra primeiro teria dado um passo para baixo e não teria revertido.

Para consertar nosso erro, precisamos garantir que só possamos mudar de direção depois maine advanceSnaketermos sido chamados. Podemos criar uma variável changingDirection. Isso será definido como true quando changeDirectionfor chamado e false quando advanceSnakefor chamado.

Dentro da nossa changeDirectionfunção, podemos retornar mais cedo se a mudança de Direcção for verdadeira.

function changeDirection(event) {  const LEFT_KEY = 37;  const RIGHT_KEY = 39;  const UP_KEY = 38;  const DOWN_KEY = 40;
  if (changingDirection) return;
  changingDirection = true;
  ...}
function main() {  setTimeout(function onTick() {    changingDirection = false;        ...
  }, 100)}
Aqui está a nossa versão final do snake.html

Observe que eu também adicionei alguns estilos? entre as lt;style><tags & ; / style>. Isso é para fazer a tela e a pontuação aparecerem no meio da tela.
