# Thread API # 

En esta sección escribiremos algunos programas multi-hilo y usaremos una herramienta específica llamada ```helgrind``` para encontrar problemas en estos programas. 

## Questions ##

1. Primero codifique ```main-race.c```. Examine el código de manera que usted pueda ver (ojalá de manera obvia) un data race en el código. Ahora ejecute ```helgrind``` (al teclear ```valgrind --tool=helgrind ./main-race```) y vea como este programa reporta los *data races*. ¿Se muestran las líneas de código involucradas?, ¿Qué otra información entrega este programa?
R/ Imprime las direcciones de la data race y se ve como el hilo 1 y el 2 están involucrados, nos muestra el contador de errores y la variable que nos está generando la data race. 

2. ¿Qué ocurre cuando usted elimina una de las líneas que generan problemas en el código? Ahora agrege un lock alrededor de las actualizaciones de la variable compartida, y entonces alrededor de ambas. ¿Qué reporta ```helgrind``` en cada uno de estos casos?
R/ En cada uno de los casos muestra simplemente que ya no existe esa condición de carrera entre los hilos, hasta llegar al punto de no mostrar ningún error cuando están todos los locks establecidos alrededor de las variables de acceso global involucradas.

3. Ahora observe ```main-deadlock.c```. Examine el código. Este código tiene un problema conocido como deadlock. ¿Puede ver que problema podrá este tener?
R/El problema que puede ocurrir es que va a llegar un punto en que no habrá nada que despierte a los hilos dormidos, y se bloquean mutuamente; cuando el hilo p1 adquiere el lock de m1 y realiza inmeadiatamente un cambio de contexto; así mismo el hilo p2 adquiere el lock de m2, luego el hilo p2 intenta adquirir el lock de m1 el cual ya se encuentra bloqueado por lo cual cambia de contexto hacia p1; y el hilo p1 procede adquirir el lock de m2, el cual ya se encuentra bloqueado por el hilo p2. Es decir, cada hilo se encuentra a la espera de un recurso que libera el otro hilo.

4. Ahora ejecute ```helgrind``` en este código. ¿Qué reporta helgrind?
R/ Helgrind reporta una violacion de orden en adquisicion de los bloqueos 


5. Ahora ejecute ```helgrind``` en ```main-deadlock-global.c```. Examine el código. ¿Tiene este el mismo problema que ```main-deadlock.c```? ¿Muestra ```helgrind``` el mismo reporte de error? ¿Qué dice esto a cerca de herramientas como ```helgrind```?
R/ Cuando analizamos el codigo se identifica que se utiliza el mutex al rededor de la seccion critica lo cual controlaria el problema que observamso en el punto anterior, sin embargo al ejecutar con la herramienta helgrind nos reporta aparentemente un error similar, lo cual no tiene sentido ya que estamos controlando el problema que se presenta en la sección critica atravez de los mutex, esto nos indica que estas herramientas no son tan confiables como aparentan ser.  


6. Ahora observe ```main-signal.c```. Este código usa una variable (```done```) para señalar que el hijo esta hecho y que el padre puede continuar. ¿Por qué este códido es ineficiente? (En que termina el padre dedicando su tiempo, si el hijo toma una gran cantidad de tiempo en completarse).
R/ Si el scheduler decide ejecutar al padre antes que el hijo logre cambiar el estado de la variable "done" entonces el padre consumira el quantum completo de la cpu o hasta que el scheduler decida hacer el cambio de contexto analizando una variable que no va a cambiar 


7. Ahora ejecute ```helgrind``` para este programa. ¿Qué reporta helgrind?, ¿Es correcto el código?
R/ El codigo tiene una sintaxis correcta y la herramienta helgrind nos informa de un posible data race sobre la variable done


**Nota**: Se adjuntan los códigos para facilitar en análisis.
