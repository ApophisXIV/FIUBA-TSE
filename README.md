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
Para lograr esto vas posiblemente vas a tener que leer bastante documentación para configurarlo o utilizar el script 
que desarrollamos en este repositorio denominado `add_prinft.py` logrando habilitar la modalidad de `semihosting`

```
Semihosting is a mechanism that enables code running on an ARM target to communicate and use
 the Input/Output facilities on a host computer that is running a debugger
```

Si de todos modos querés hacerlo de forma manual podes empezar por acá 
- ("Documentacion oficial (casi)") https://community.st.com/t5/stm32-mcus/how-to-configure-stm32-vs-code-extension-to-use-openocd/ta-p/748562
- (Recomendaciones para hacerlo andar en VSCode) https://community.st.com/t5/stm32-vscode-extension-mcus/any-hints-on-using-swo-output-from-stm32-within-vscode-via/td-p/704896
- (Otras opciones que podes explorar) https://merkles.com/wiki/index.php/STM32_-_How_To#ARM_-_STM32_Semihosting
- https://electronics.stackexchange.com/questions/149387/how-do-i-print-debug-messages-to-gdb-console-with-stm32-discovery-board-using-gd
- (Los flags para gcc-arm) https://community.st.com/t5/stm32cubeide-mcus/how-to-get-arm-semihosting-to-work-on-stm32cubeide/td-p/302567
- (Idea de configuración del launch.json) https://electronics.stackexchange.com/questions/448019/live-variable-view-using-openocd-and-vscode
- (Argumentos en launch.json) https://stackoverflow.com/questions/75703331/how-to-add-arguments-to-launch-json-in-vs-code
- (¿A donde van los datos de GDB en modo semihosting sobre TCP/IP?) https://stackoverflow.com/questions/58126134/where-does-stdout-go-with-gdb-oopenocd-and-semihosting
- (Semihosting at-runtime) https://electronics.stackexchange.com/questions/302193/is-there-a-way-to-check-if-semihosting-is-enabled-at-runtime
- (Un resumen de semihosting) https://shawnhymel.com/1840/how-to-use-semihosting-with-stm32/
- (Habilitar modo semihosting) https://stackoverflow.com/questions/61332036/debug-semihosting-printf-function-on-stm32
- (Libncurses - Solo en linux) https://www.reddit.com/r/embedded/comments/1emxzkj/problems_using_stm32cubeide_on_linux/?tl=es-es
- (Syscalls del sistema) https://community.st.com/t5/stm32-mcus-products/role-of-syscalls-c/td-p/270881
- (When using -specs=rdimon ... make sure to not use -specs=nosys.specs) https://www.openstm32.org/forumthread2141
- (GDB + Semihosting o ITM + ST-Link) https://www.openstm32.org/forumthread2893
- (Una gran guía para habilitar el printf en STM32 por UART - No por semihosting) https://www.reddit.com/r/embedded/comments/1bby3qt/the_definitive_guide_to_enabling_printf_on_an/
- (Printf sobre UART) https://www.youtube.com/watch?v=WnCpPf7u4Xo&ab_channel=ViduraEmbedded
- (Habilitando printf mediante ITM) https://community.st.com/t5/stm32cubeide-mcus/enable-itm-printf-output-using-openocd-gdb-on-stm32h7/td-p/768537
- ("Error: Can't find interface/stlink.cfg") https://community.st.com/t5/stm32-mcus-products/nucleo-stm32h5-debugging-openocd/td-p/618225/page/2

Esta playlist me fue de gran utilidad para buscar las variables de entorno necesarias para lograr el objetivo y recorrer la configuración general del proyecto
- (Una gran playlist sobre CMake + STM32 + VSCode - Esta en ruso) https://www.youtube.com/playlist?list=PL3hso3baaaPz4NgZW7kzAn0LzHdhCoQry
  
En este video vemos la configuracion general mediante la extensión oficial de VSCode para STM32 (Muy recomendado)
- https://www.youtube.com/watch?v=ADvPoRwRyQA&ab_channel=ALCSmartSystems

## ¿Usas el ST-Link V1/V2?
En caso de ser afirmativa la respuesta vas a tener que tener en cuenta otras cosas como por ejemplo modificar la placa y agregar el pin que falta para poder tener una conexión SWO
Te recomiendo este video y este articulo:
- https://www.youtube.com/watch?v=JqrUAzjJ0tw
- https://www.phippselectronics.com/debug-an-stm32-with-printf-using-only-an-st-link/

## Ejecutando el debugger y utilizando printf
Es importante que en el main.c agregues la biblioteca estandar para el manejo de salida del programa `#include <stdio.h>` y que definas e invoques la funcion que se va a encargar de manejar la comunicación en modo semihosting entre el target (el uC) y el host (la PC)

**Definición**  
![definicion_funcion_monitor](https://github.com/user-attachments/assets/745617e5-b41c-4516-ad21-45c653519116)

**Implementación**  
![uso_funcion_monitor](https://github.com/user-attachments/assets/8e340526-49b5-45db-915f-092159c4a011)

**Uso del printf**  
![uso_printf](https://github.com/user-attachments/assets/44c7684f-0d14-4281-b761-39ab4eba0d7e)


