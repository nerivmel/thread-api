# Thread API # 

En esta sección escribiremos algunos programas multi-hilo y usaremos una herramienta específica llamada ```helgrind``` para encontrar problemas en estos programas. 

## Questions ##

1. Primero codifique ```main-race.c```. Examine el código de manera que usted pueda ver (ojalá de manera obvia) un data race en el código. Ahora ejecute ```helgrind``` (al teclear ```valgrind --tool=helgrind ./main-race```) y vea como este programa reporta los *data races*. ¿Se muestran las líneas de código involucradas?, ¿Qué otra información entrega este programa?


2. ¿Qué ocurre cuando usted elimina una de las líneas que generan problemas en el código? Ahora agrege un lock alrededor de las actualizaciones de la variable compartida, y entonces alrededor de ambas. ¿Qué reporta ```helgrind``` en cada uno de estos casos?


3. Ahora observe ```main-deadlock.c```. Examine el código. Este código tiene un problema conocido como deadlock. ¿Puede ver que problema podrá este tener?


4. Ahora ejecute ```helgrind``` en este código. ¿Qué reporta helgrind?


5. Ahora ejecute ```helgrind``` en ```main-deadlock-global.c```. Examine el código. ¿Tiene este el mismo problema que ```main-deadlock.c```? ¿Muestra ```helgrind``` el mismo reporte de error? ¿Qué dice esto a cerca de herramientas como ```helgrind```?


6. Ahora observe ```main-signal.c```. Este código usa una variable (```done```) para señalar que el hijo esta hecho y que el padre puede continuar. ¿Por qué este códido es ineficiente? (En que termina el padre dedicando su tiempo, si el hijo toma una gran cantidad de tiempo en completarse).


7. Ahora ejecute ```helgrind``` para este programa. ¿Qué reporta helgrind?, ¿Es correcto el código?


**Nota**: Se adjuntan los códigos para facilitar en análisis.
