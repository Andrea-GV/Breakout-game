Pasos para crear el juego:

1.  He creado la <b>estructura html</b>: title, style básico, canvas y luego el script

2.  En el Script <b>recuperar</b> el canvas, el contexto para pintar por encima y establecer el tamaño del canvas

3.  Crear la <b>función para pintar</b> la pantalla (function draw), que se ejecutará en cada frame o cada vez que se refresque o haya un cambio en la pantalla (UN LOOP). En vez de hacer un setTimeOut para que se ejecute cada X segundos, el requestAnimationFrame se ejecutará cada segundo (60 veces x min). Para ello:

    - creo la <b>función draw</b>
    - incluyo todas las funciones de dibujo de elementos y chequeo de colisiones y movimientos
    - finaliza la función repintando con window.requestAnimationFrame(draw);

4.  Declaro las <b>variables</b> de la pelota (el tamaño, posición, dirección y velocidad)

5.  A la <b>function drawBall</b> le indico:

    - comienza
    - cómo es mi elemento pelota usando arc:
      - - Para dibujar una pelota necesitamos usar las coordenadas de posición (x e y), el tamaño (ballRadius), el angulo de inicio (0) y el angulo de final (pi\*2 para que sea una circunferencia)
      - - color de la pelota
      - - pinta la pelota
      - - cierra el trazado para mejorar el render

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

\*\*\* Como no se estaba ejecutando el reload. He tenido que añadir un setTimeout y otro if interno para comprobar si !gameOver inicializado en false, ejecutase el reload.\*\*\*

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

14. Aplicar colisión de la pelota en el paddle para que rebote y pueda jugar. Para ello, debo hacer modificaciones en la función ballMovement

- Al game over si cae abajo del canva, le tengo cambio el if por un else if
- Creo un if para el rebote al tocar el paddle
- Creo una constante que sea un boolean, para comprobar si la pelota está en la misma posición (es decir, toca) vertical u horizontal que la paddle. Para ello:
  - - isBallSameXAsPaddle define si la x (posición horizontal de la pelota) es menor que la paddleX (posición horizontal de la paddle) && la x es mayor que la paddleX + su ancho.
      Es decir, comprueba que la pelota esté en un "marco" igual que la paddle
  - - isBallTouchingPaddle define si y (posición de la pelota) + dy (dirección de la misma) es mayor que paddleY (altura del paddle)
- Ahora al if le digo que compruebe AMBAS constantes (posición y tocar) para que la bola cambie de dirección en caso de que sea TRUE --> Le dé a la paddle
