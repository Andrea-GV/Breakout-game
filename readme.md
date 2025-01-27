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
    Para ello, he creado la <b>function clearCanvas</b>

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

10. <b>Crear el paddle</b> con el que jugar.
    - Declaro las variables que definen el tamaño de la paddle
    - Su posición horizontal inicial en la pantalla ( en el centro )
    - Y la vertical (para que no esté abajo del todo)
11. <b>Dibujar el paddle</b>. En vez de usar la técnica para la pelota, uso fillRect (que dibuja rectángulos) al que le defino en qué coordenada posicionarlo (X e Y), y defino el width y el height. Como ya he declarado variables con esos tamaños, las invoco.
