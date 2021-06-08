# Creando una REST API en menos de 1 hora con Oracle Database Actions

## Pre-requisitos

- Una instancia de ADB-S (Puede ser tipo "Always Free")
- Un navegador de internet
- Una terminal tipo UNIX (Puede ser "Git Bash" en Windows)
- cURL debe estar instalado en la terminal tipo UNIX que se pretenda utilizar
- Opcionalmente tener instalado `jq`, que es una herramienta de línea de comandos para formatear y filtrar JSON

## Propósito

Este documento te guiará paso a paso por la creación de una REST API para una aplicacón ficticia de recordatorios (TODOs en inglés) que tiene dos tablas:

- `USERS`: Donde almacenaremos nuestros usuarios
- `TODOS`: Donde almacenaremos nuestras tareas

En este documento proporcionaremos dos archivos con datos de ejemplo para generar las tablas que necesitamos y posteriormente procederemos a habilitar las tablas para REST usando Oracle Database Actions.

## Crear un nuevo usuario (Paso a Paso)

1. Accede a la consola de servicio de tu instancia de Autonomous Database
2. Selecciona la tab **Tools** y posteriormente haz click en **Open Database Actions**
    ![](./assets/01.png)
3. Ingresa `ADMIN` y haz click en **Siguiente**
    ![](./assets/02.png)
4. Ingresa tu password y haz click en **Conectar**
    ![](./assets/03.png)
5. Abre **Usuarios de Base de Dato**
    ![](./assets/04.png)
6. Haz click en **Crear usuario**
    ![](./assets/05.png)
7. Llena los detalles del usuario y haz click en **Crear Usuario**. Es muy importante habilitar la opción de **Activación REST** y asignarle una cuota diferente a la predeterminada al usuario, de lo contrario no podremos cargar datos al esquema
    ![](./assets/06.png)
8. El nuevo usuario aparecera en el reporte una vez creado correctamente
    ![](./assets/07.png)
9. Haz click en el botón de usuario de la esquina superior derecha y después haz click en **Cerrar sesión**
    ![](./assets/08.png)

## Carga de Datos (Paso a paso)

1. Descarga el archivo [users.csv](./assets/users.csv) del repositorio
2. Descarga el archivo [todos.csv](./assets/todos.csv) del repositorio
3. Haz click en **Conectar**
    ![](./assets/09.png)
4. Ingresa el nuevo nombre de usuario que creaste anteriormente y haz click en **Siguiente**
    ![](./assets/10.png)
5. Ingresa el password y haz click en **Conectar**
    ![](./assets/11.png)
6. Haz click en **SQL**
    ![](./assets/12.png)
7. En el panel inferior, haz click en **Carga de datos**
    ![](./assets/13.png)
8. Haz click en el botón con el ícono de nube
    ![](./assets/14.png)
9. Haz click en **Seleccionar Archivos**
    ![](./assets/15.png)
10. Selecciona el archivo **users.csv** y haz click en **Abrir**
    ![](./assets/16.png)
11. Se desplegará una previsualización de los datos, haz click en **Siguiente**
    ![](./assets/17.png)
12. Cambia el `ID` a tipo `NUMBER`, selecciona la opción `PK` y deselecciona la opción `NULL`, luego haz click en **Siguiente** 
    ![](./assets/18.png)
13. Se mostrará una vista previa de los comandos a ejecutarse. Haz click en **Finalizar**
    ![](./assets/19.png)
14. Aparecerá un diálogo indicándonos que la carga de datos continuará ejecutándose en el fondo. Haz click en **Aceptar**
    ![](./assets/20.png)
15. En el panel de abajo, debe aparecernos nuestra carga reciente en el reporte, indicando que no hubo fallos
    ![](./assets/21.png)
16. Haz click nuevamente en el botón con el ícono de nube
    ![](./assets/22.png)
17. Haz click en **Seleccionar Archivos**
    ![](./assets/23.png)
18. Selecciona el archivo **todos.csv** y haz click en **Abrir**
    ![](./assets/24.png)
19. Se mostrará una vista previa de los datos, haz click en **Siguiente**
    ![](./assets/25.png)
20. Cambia los setings como se indica debajo y luego haz click en **Siguiente**
    - Para `ID`, cambia el tipo de datos a `NUMBER`, selecciona la opción `PK` y deselecciona la opcion `NULL`
    - Para `ASIGNEE` cambia el tipo de datos a `NUMBER` y deselecciona la opcion `NULL`
    - Para `CREATED_BY` cambia el tipo de datos a `NUMBER` y deselecciona la opcion `NULL`
    ![](./assets/26.png)
21. Se mostrará una previsualización del código a ejecutar, haz click en **Siguiente**
    ![](./assets/27.png)
22. Se mostrará un diálogo indicando que la subida de datos continuará en el background. Haz click en **Aceptar**
    ![](./assets/28.png)
23. Verifica que en el reporte del panel inferior se muestre la nueva carga de datos con cero errores
    ![](./assets/29.png)

## Verificación de los datos (Paso a paso)

1. Arrastra la tabla **USERS** desde el panel izquierdo hacia el editor, aparecerá un diálogo. Selecciona la opción **Seleccionar** y luego haz click en **Aplicar**
    ![](./assets/30.png)
2. Se creará una consulta y se pondrá en el editor, haz click en botón con el icono de reproducción para ejecutar la consulta
    ![](./assets/31.png)
3. Se mostrarán los resultados en el panel inferior
    ![](./assets/32.png)
4. Haz click en el botón con el icono de basurero para borrar el contanido del editor
    ![](./assets/33.png)
5. Arrastra la tabla **TODOS** desde el panel izquierdo hacia el editor, aparecerá un diálogo. Selecciona la opción **Seleccionar** y luego haz click en **Aplicar**
    ![](./assets/34.png)
6. Se creará una consulta y se pondrá en el editor, haz click en botón con el icono de reproducción para ejecutar la consulta
    ![](./assets/35.png)
7. Se mostrarán los resultados en el panel inferior
    ![](./assets/36.png)

## Creación de los "endpoints" REST (Paso a paso)

1. Haz click derecho en la tabla de **USERS**, lusgo selecciona la opción **REST** y luego la opción **Activar**
    ![](./assets/37.png)
2. Se mostrará un panel para activar la tabla como "endpoint" REST, haz click en **Activar**. 

    Esto creará un "endpoint" REST con soporte para los métodos `GET`, `POST`, `PUT` y `DELETE` de forma automática, los cuales probaremos más adelante.
    
    Nota que la opción **Necesita Autentificación** está desactivada para simplificar este ejemplo, pero dependiendo de tus necesidades necesitarás activarla. Los siguientes pasos asumen que esta opción está desactivada
    ![](./assets/38.png)
3. Una vez activada la tabla como "endpoint" REST se mostrará un icono de clavija en la tabla, indicando que la tabla esta activada.
    ![](./assets/39.png)
4. Haz click derecho en la tabla **TODOS** en el panel izquierdo, selecciona la opción **REST** y posteriormente selecciona la opción **Activar**
    ![](./assets/40.png)
5. Se mostrará un panel, haz click en **Activar**.
    ![](./assets/41.png)
6. Una vez activada la tabla como "endpoint" REST se mostrará un icono de clavija en la tabla, indicando que la tabla esta activada.
    ![](./assets/42.png)

## Probando los "endpoints" REST con cURL

1. Haz click derecho en la tabla **USERS** del panel izquierdo, selecciona la opcion **REST** y posteriormente la opción **Comando cURL...**
    ![](./assets/43.png)
2. Se abrirá un panel del lado derecho que nos mostrará distintas tabs en vertical con las acciones que podemos realizar. Por defecto, la tab **GET All** esta seleccionada. Haz click en el botón con el icono de copiar
    ![](./assets/44.png)
3. Abre una terminal, en Windows puedes usar **Git Bash**, luego pega el comando que copiaste en la terminal, si tienes `jq` instalado, puedes añadir 
    ```sh
    | jq
    ```
    para formatear el JSON que recibamos del endpoint, de otro modo el JSON aparecerá sin espacios u otro separador.

    Observa que el JSON contiene hasta 25 filas de la tabla y además contiene los links en la parte de abajo para paginar el recurso
    ![](./assets/46.png)
4. Regresa a Database Actions, selecciona la tab **GET Simple** y rellena el campo de **id** con `1`, luego haz click en el botón con el icono de copiar
    ![](./assets/47.png)
5. En la terminal, pega el comando que copiaste, si tienes `jq` instalado, puedes añadir 
    ```sh
    | jq
    ```
    para formatear el JSON que recibamos del endpoint, de otro modo el JSON aparecerá sin espacios u otro separador.

    Observa que el JSON contiene una sola fila, y que el final de la URL termina con un valor de la columna `ID` de la tabla, esto debido a que en la tabla, indicamos que nuestro `PK` (Primary Key) era esa misma columna
    ![](./assets/48.png)
6. Regresa a Database Actions, selecciona la tab **POST**, rellena el campo de **id** con `20` (Ya que no existe en nuestro data set), después rellena los demás campos y haz click en el botón con el icono de copiar
    ![](./assets/49.png)
7. En la terminal, pega el comando que copiaste, si tienes `jq` instalado, puedes añadir 
    ```sh
    | jq
    ```
    para formatear el JSON que recibamos del endpoint, de otro modo el JSON aparecerá sin espacios u otro separador.

    Observa que el método POST insertará una fila y que el JSON devuelto contiene la fila recién creada
    ![](./assets/50.png)
8. Regresa a Database Actions, selecciona la tab **PUT** y cambia el campo de **email** con un valor distinto al actual, luego haz click en el botón con el icono de copiar
    ![](./assets/51.png)
9. En la terminal, pega el comando que copiaste, si tienes `jq` instalado, puedes añadir 
    ```sh
    | jq
    ```
    para formatear el JSON que recibamos del endpoint, de otro modo el JSON aparecerá sin espacios u otro separador.

    Observa que el método PUT actualizará una fila y que el JSON devuelto contiene la fila recién actualizada
    ![](./assets/52.png)
10. Regresa a Database Actions, selecciona la tab **DELETE** y asegurate que el campo de **id** contenga el valor `20`, luego haz click en el botón con el icono de copiar
    ![](./assets/53.png)
11. En la terminal, pega el comando que copiaste, si tienes `jq` instalado, puedes añadir 
    ```sh
    | jq
    ```
    para formatear el JSON que recibamos del endpoint, de otro modo el JSON aparecerá sin espacios u otro separador.

    Observa que el método DELETE eliminará una fila y que el JSON devuelto contiene el número de filas eliminadas
    ![](./assets/54.png)
12. Regresa a Database Actions. Haz click en el boton **Cerrar** del panel que aparece a la izquierda y luego haz click derecho en la tabla **TODOS** del panel izquierdo, selecciona la opcion **REST** y posteriormente la opción **Comando cURL...**
    ![](./assets/55.png)
13. El panel de cURL se abrirá en la tab **GET All**, haz click en el botón con el icono de copiar
    ![](./assets/56.png)
14. En la terminal, pega el comando que copiaste, si tienes `jq` instalado, puedes añadir 
    ```sh
    | jq
    ```
    para formatear el JSON que recibamos del endpoint, de otro modo el JSON aparecerá sin espacios u otro separador.

    Observa que el JSON contiene hasta 25 filas de la tabla y además contiene los links en la parte de abajo para paginar el recurso
    ![](./assets/57.png)
15. Regresa a Database Actions, selecciona la tab **GET Simple** y rellena el campo de **id** con `1`, luego haz click en el botón con el icono de copiar
    ![](./assets/58.png)
16. En la terminal, pega el comando que copiaste, si tienes `jq` instalado, puedes añadir 
    ```sh
    | jq
    ```
    para formatear el JSON que recibamos del endpoint, de otro modo el JSON aparecerá sin espacios u otro separador.

    Observa que el JSON contiene una sola fila, y que el final de la URL termina con un valor de la columna `ID` de la tabla, esto debido a que en la tabla, indicamos que nuestro `PK` (Primary Key) era esa misma columna
    ![](./assets/59.png)
17. Regresa a Database Actions, selecciona la tab **POST**, rellena el campo de **id** con `110` (Ya que no existe en nuestro data set), el campo **asignee** con `1` y el campo **created_by** con `1`, después rellena los demás campos y haz click en el botón con el icono de copiar
    ![](./assets/60.png)
18. En la terminal, pega el comando que copiaste, si tienes `jq` instalado, puedes añadir 
    ```sh
    | jq
    ```
    para formatear el JSON que recibamos del endpoint, de otro modo el JSON aparecerá sin espacios u otro separador.

    Observa que el método POST insertará una fila y que el JSON devuelto contiene la fila recién creada
    ![](./assets/61.png)
19.  Regresa a Database Actions, selecciona la tab **PUT** y cambia el campo de **completed** con un valor distinto al actual, luego haz click en el botón con el icono de copiar
    ![](./assets/62.png)
20. En la terminal, pega el comando que copiaste, si tienes `jq` instalado, puedes añadir 
    ```sh
    | jq
    ```
    para formatear el JSON que recibamos del endpoint, de otro modo el JSON aparecerá sin espacios u otro separador.

    Observa que el método PUT actualizará una fila y que el JSON devuelto contiene la fila recién actualizada
    ![](./assets/63.png)
21. Regresa a Database Actions, selecciona la tab **DELETE** y asegurate que el campo de **id** contenga el valor `110`, luego haz click en el botón con el icono de copiar
    ![](./assets/64.png)
22. En la terminal, pega el comando que copiaste, si tienes `jq` instalado, puedes añadir 
    ```sh
    | jq
    ```
    para formatear el JSON que recibamos del endpoint, de otro modo el JSON aparecerá sin espacios u otro separador.

    Observa que el método DELETE eliminará una fila y que el JSON devuelto contiene el número de filas eliminadas
    ![](./assets/65.png)

## Conclusión

Al terminar las secciones de paso a paso anteriores, habremos aprendido:

- Cómo cargar datos mediante Database Actions
- Cómo mostrar el reporte de datos de una tabla usando Database Actions
- Cómo habilitar tablas como "endpoints" REST para su acceso mediante cURL