# FIUBA - TSE
## Usando VSCode para la materia
Si preferis utilizar VSCode en vez de STM32CubeIDE tenés que seguir una serie de pasos

## Instalar la extensión oficial
Podes guiarte con estos videos:
- https://www.youtube.com/watch?v=DDVdq47Dd94&ab_channel=STMicroelectronics
- https://www.youtube.com/watch?v=aLD9zggmop4&ab_channel=PRTechTalk
Lo importante es que tengas bien configuradas las rutas del compiler y cubemx

## Modificar la variable de entorno de CMake `Configure`
En una serie de pasos tenés que configurar el entorno en modo debug (esto añade flags de depuración al momento de compilar)
lo que te permitirá hacer debug y agregar breakpoints mediante GDB

![configuracion_vscode_debug_cmake](https://github.com/user-attachments/assets/b5a7afa0-50f0-4265-86bb-b8645b45a178)

## Configurando el servidor de OpenOCD para debug y semihosting
De esta forma vas a poder realizar debugging mediante OpenOCD (https://openocd.org/) y permitir
que el target utilice la función `printf(...)`. 
Para lograr esto vas a tener que leer bastante documentación para configurarlo o utilizar el script 
que desarrollamos en este repositorio denominado `add_prinft.py`.
Si de todos modos querés hacerlo de forma manual podes empezar por acá (https://community.st.com/t5/stm32-mcus/how-to-configure-stm32-vs-code-extension-to-use-openocd/ta-p/748562)

## Ejecutando el debugger y utilizando printf
Es importante que en el main.c agregues la biblioteca estandar para el manejo de salida del programa `#include <stdio.h>` y que definas e invoques la funcion que se va a encargar de manejar la comunicación en modo semihosting entre el target (el uC) y el host (la PC)

**Definición**  
![definicion_funcion_monitor](https://github.com/user-attachments/assets/745617e5-b41c-4516-ad21-45c653519116)

**Implementación**  
![uso_funcion_monitor](https://github.com/user-attachments/assets/8e340526-49b5-45db-915f-092159c4a011)

**Uso del printf**  
![uso_printf](https://github.com/user-attachments/assets/44c7684f-0d14-4281-b761-39ab4eba0d7e)


