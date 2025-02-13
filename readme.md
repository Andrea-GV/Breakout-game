Pasos para crear el juego:

1.  He creado la <b>estructura html</b>: title, style básico, canvas y luego el script

2.  En el Script <b>recuperar</b> el canvas, el contexto para pintar por encima y establecer el tamaño del canvas

3.  Crear la <b>función para pintar</b> la pantalla (function draw), que se ejecutará en cada frame o cada vez que se refresque o haya un cambio en la pantalla (UN LOOP). En vez de hacer un setTimeOut para que se ejecute cada X segundos, el requestAnimationFrame se ejecutará cada segundo (60 veces x min). Para ello:

    - creo la <b>función draw</b>
    - incluyo todas las funciones de dibujo de elementos y chequeo de colisiones y movimientos
    - finaliza la función repintando con window.requestAnimationFrame(draw);

4.  Declaro las <b>variables</b> de la pelota (el tamaño, posición, dirección y velocidad)

5.  A la <b>function drawBall</b> le indico:

    - Comienza
    - Cómo es mi elemento pelota usando arc:
      - - Para dibujar una pelota necesitamos usar las coordenadas de posición (x e y), el tamaño (ballRadius), el angulo de inicio (0) y el angulo de final (pi\*2 para que sea una circunferencia)
      - - Color de la pelota
      - - Pinta la pelota
      - - Cierra el trazado para mejorar el render

6.  Comienzo a definir el movimiento de la pelota <b>function ballMovement</b> con un movimiento lateral y vertical

7.  Al definir el movimiento no elimina cada movimiento anterior, con lo que hace una trazada, la trayectoria completa.
    Para ello, he creado la <b>function cleanCanvas</b> que elimina el rastro de cada frame anterior.

8.  A la <b>function draw</b> le defino la limpieza del canvas:

    - Desde la posición 0 de la x
    - La posición 0 de la y
    - Hasta la posición del ancho del canvas
    - Y la posición del alto del canvas
      Es decir, limpia TODOS los dibujos del canvas

9.  <b>Definir las primeras colisiones</b>:

    - A <b>function ballMovement</b> le he añadido delante de la definición del movimiento, el rebote de la pelota en las paredes de los laterales y superior.

    Para ello:

    - Añado un if (si choca) que cambie la orientación del movimiento. Es decir, deja de ir a dcha y pasa a ir a izq
    - En el 1er if he añadido la condición:

      - - Si la posición de la pelota (x) + la dirección (dx)
      - - Es mayor que el ancho del canvas (canvas.width) - el tamaño de la pelota (ballRadius)
      - - O es menor que éste
      - - > > Cambia de dirección

    - En el 2º if he añadido la condición:

      - - Si la posición de la pelota (y) + la dirección (dy)
      - - Es menor que el tamaño de la pelota (ballRadius)
      - - > > Cambia de dirección

    - En el 3er if he añadido la condición:
      - - Si la posición de la pelota (y) + la dirección (dy)
      - - Es mayor que el alto del canvas (canvas.height) - el tamaño de la pelota (ballRadius)
      - - > > Log de acaba el juego
      - - > > Recarga

**Como no se estaba ejecutando el reload. He tenido que añadir un setTimeout y otro if interno para comprobar si !gameOver inicializado en false, ejecutase el reload.**

10. <b>Crear el paddle</b> con el que jugar.

    - Declaro las variables que definen el tamaño de la paddle
    - Su posición horizontal inicial en la pantalla ( en el centro )
    - Y la vertical (para que no esté abajo del todo)

11. <b>Dibujar el paddle en function drawPaddle</b>. En vez de usar la técnica para la pelota, uso fillRect (que dibuja rectángulos) al que le defino en qué coordenada posicionarlo (X e Y), y defino el width y el height. Como ya he declarado variables con esos tamaños, las invoco.

12. Creo la función <b>initEvents</b>para recoger el evento (movimiento del paddle) del teclado.

    - Recojo cuando se presiona (keydown)
    - Cuando suelta (keyup)

    - Creo las variables rightPressed y leftPressed para recoger esta información y poder actualizar el estado de la paddle en función de qué tecla se ha pulstado. Las inicializo en false.

    - Creo la función <b>keyDownHandler</b> que recoge la info de la tecla pulsada y cambia a true para que se mueva el paddle
    - Creo la función <b>keyUpHandler</b> que recoge la info de la tecla soltada y cambia a false de nuevo

13. Comienzo a definir la función <b>paddleMovement</b> :

- Si se presiona la tecla derecha, mueve el paddle hacia la derecha
- Si se presiona la tecla izq, mueve el paddle a la izq
- Ambos tienen un ternario para tener en cuenta la pared del canva, el ancho del canva y el tamaño del paddle, que sea el máximo de su movimiento y no desaparezca de la pantalla de juego
- Añadido la variable paddle_sensibility que define la velocidad de movimiento del paddle. En vez de dejarlo como valor fijo en la función, he preferido añadir esta variable por si en un futuro quiero poder aumentar la velocidad del movimiento según pase el tiempo, queden menos ladrillos o cualquier otra función que complique un poco la jugabilidad.

14. Aplicar <b>colisión de la pelota en el paddle</b> para que rebote y pueda jugar. Para ello, debo hacer modificaciones en la función <b>ballMovement</b>

- Al game over si cae abajo del canva, le tengo cambio el if por un else if
- Creo un if para el rebote al tocar el paddle
- Creo una constante que sea un boolean, para comprobar si la pelota está en la misma posición (es decir, toca) vertical u horizontal que la paddle. Para ello:
  - - isBallSameXAsPaddle define si la x (posición horizontal de la pelota) es menor que la paddleX (posición horizontal de la paddle) && la x es mayor que la paddleX + su ancho.
      Es decir, comprueba que la pelota esté en un "marco" igual que la paddle
  - - isBallTouchingPaddle define si y (posición de la pelota) + dy (dirección de la misma) es mayor que paddleY (altura del paddle)
- Ahora al if le digo que compruebe AMBAS constantes (posición y tocar) para que la bola cambie de dirección en caso de que sea TRUE --> Le dé a la paddle

15. <b>Estilar</b> un poco el juego.

- Añadido el fondo del juego (un sprite que se repite en todo el canva) aplicando al canvas el background
- Antes del del script, he añadido dos etiquetas de img:
  - - La 1ª img es el set entero de sprites del juego que más adelante utilizaré para coger la imagen del paddle
  - - La 2ª img es una línea de bricks
- Ambas tienen el display none con el hidden
- Ambas tienen id para poder capturarlo después (querySelector)

Al inicio del script antes de definir el tamaño del canvas, he añadido las constantes para capturar los id de las imágenes ($sprite y $bricks)

- - querySelector y getElementById en este caso actúan igual

15. 1. <b>Cambiar el paddle por el sprite</b> en vez de un rectángulo pintado.

- A la function drawPaddle cambio esta manera de pintarlo
  ctx.fillStyle = 'darkred';
  ctx.fillRect(
  paddleX,
  paddleY,
  paddleWidth,
  paddleHeight);

- Añadiendo ctx.drawImage(). Dentro le defino:
  - 1º tengo que decirle qué imagen coge ($sprite)
  - 2º El clipX --> La coordenada desde donde debe empezar a recortar la imagen
  - 3º El clipY --> donde finaliza la imagen
    - - Para saber las medidas he abierto la png (desde editor de fotos explorador archivos) y con el cursor del ratón buscar el pixel desde donde comienza a dibujar el paddle
    - - Cojo la 1ª coordenada del cursor (X) y la 2ª (Y). La primera es el ancho desde el lateral izq de inicio de img y la 2ª el alto de la misma
- 4º Defino el ancho del recorte de img utilizando la variable ya declarada con esta medida
- 5º Defino el alto del recorte de img, utilizando la variable ya declarada con esta medida
- 6º Le digo dónde debe ubicar la imagen.
  - - En el paddleX q se declaró antes
  - - En el paddleY
- 7º El ancho y alto del dibujo (img). Que en este caso siguen siendo
  - - paddleWidth
  - - paddleHeight
      Podría poner otros números distintos y el tamaño del paddle cambiaría (deformaría la imagen). En este caso, el tamaño del paddle es el mismo tamaño que el dibujo.

16. <b>Añadir los bricks</b>

- 1º Declarar variables para definir cuántas filas y columnas de ladrillos deben aparecer (bricksRows, bricksCols)
- 2º Definir el ancho y alto de los ladrillos (bricksWidth, bricksHeight)
- 3º Definir la separación entre cada ladrillo (bricksPadding)
- 4º Definir la posición/dónde empiezan los bricks en el canva (bricksOffsetTop, bricksOffsetLeft). Es decir el margen superior y lateral izquierdo del bloque entero de ladrillos.
- 5º Definir el array que contendrá todos los ladrillos

17. <b>Comienza el bucle para dibujar los bricks en el canva.</b>

- Con un bucle for en el que C es col y r es row
- 1º El bucle <b>for c</b> debe dibujar las columnas. Dentro de cada columna, recogerá su fila. Por eso hay un bucle anidado dentro de otro.
- 2º <b>bricks[c]</b> (dentro de la columna) lo inicializo vacío. Según recorre el bucle, pintará cada fila de ladrillos en su columna.
- 3º <b>for r</b> debe dibujar la fila en esta columna.
  - 3.1 <b>Calcular la posición horizontal de ladrillo en su columna</b> (brickX).
    Multiplico el nº de la columna (c) X [el ancho del ladrillo (bricksWidth) + la separación entre ladrillos (bricksPadding)] + la posición horizontal del bloque de ladrillos (bricksOffsetLeft) que es el margin izq de todas las columnas
  - 3.2 <b>Calcular la posición vertical de cada ladrillo en su columna</b> (brickY).
    Ocurre igual, que en el anterior cálculo pero teniendo en cuenta su altura.
    Multiplico el nº de la fila (r) X [el alto del ladrillo (bricksHeight) + la separación entre ladrillos (bricksPadding)] + la posición vertical del bloque de ladrillos
  - 3.3 <b>Asignar un color aleatorio a cada ladrillo</b>
    Al ver el sprite del brick, veo que hay 8 bricks diferentes. Quiero que dibuje de manera aleatoria cada color de ladrillo en cada fila.
    Para obtener un número aleatorio del 0 al 7 aplico:
    **Math.floor()** --> redondea a la baja devolviendo el nº entero más grande de todos, siendo menor o igual que el nº dado
    **(Math.random() X 8)** (7 será el máximo. **Si quisiera tener en cuenta 8 haría (Math.random() X 8) + 1**)
  - 3.4 <b>Creo un objeto para definir el estado del ladrillo</b> para poder comprobar si está activo que lo pinte, si lo he roto con el paddle, que no lo pinte.
  - 3.5 <b>Guardar la info de cada ladrillo</b>
    **Para cada nº de col y de row (bricks[c][r]) creamos un objeto ({}) donde la x es brickX, y es brickY, el estado (roto o entero), el color (random)**
    Es decir, el ladrillo que se encuentra en esta posición exacta (c r) debe dibujarse si está en brick_status.active y el color asignado de manera aleatoria por la función del Math previa.
