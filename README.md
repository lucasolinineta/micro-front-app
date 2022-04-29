# Micro Front End JS

    > git clone git@github.com:lucasolinineta/micro-front-app.git
    > cd micro-front-app
    > cd packages

#### Recordar que cada XAPP debe correr por si sola, pero el container requiere que las XAPP ya esten corriendo

    > cd dashboard (RUN IN VUE.JS)
    > npm install
    > npm start

Ver http://localhost:8083/

    > cd auth
    > npm install
    > npm start

Ver http://localhost:8082/auth/signin / http://localhost:8082/auth/signup

    > cd ..
    > cd dashboard
    > npm install
    > npm start

Ver http://localhost:8081/

    > cd ..
    > cd container
    > npm install
    > npm start

Ver http://localhost:8080/

### GUIA DE DESARROLLO

    1.  Designar container como host y xapps como remotos
    2.  En el remoto decidir que módulos vamos a disponer para otros proyectos
    3.  Set up module federation plugin para exponer los archivos
    4.  En el host decidir cuales módulos recibir del remoto
    5.  Setup federation plug para fetch de estos archivos
    6.  En el host refactorizar el punto de entrada para cargar de manera async
    7.  En el host importar los archivos que necesitemos desde los remotos

### COMPARTIENDO DEPENDENCIAS

    1.  Container fetch products remoteEntry.js
    2.  Container fetch cart remoteEntry.js
    3.  Container nota que ambos usan la misma dep
    4.  Container puede elegir cargar solo una copia de la dep para ambos fronts
    5.  Una sola copia se dispone para todos los front
    6.  Exponer share de ambos mf en plug de module federation (webpack.config.js)
    7.  Para fixear el uso de la dep compartida solo tenemos q cargar el index de la xapp de manera async import(“./boostrap”)
    8.  En caso de necesitar diferentes versiones de la misma dependencia, solo basta con declárala en sus respectivos package.json.
    9.  Si se declara como sigleton, pase lo que pase va a cargar solo una dependencia

### FUNNY GOTCHA

Evitar usar el mismo nombre para la xapp que valores de id dentro del html del contamina

### DEPLOYMENT

    1.  Necesitamos deployar cada microfront independientemente, incluyendo el container
    2.  La locación de los archivos remoteEntry.js de cada app se se tiene q saber en el build time.
    3.  Muchas soluciones de deploy de front asumen que estas deployando un proyecto único, necesitamos una estrategia que pueda manejar multiples soluciones
    4.  Probablemente se necesite CI/CD pipeline.

### COMPARTIENDO CSS (CSS scoping solutions)

#### CSS custom escrito por nosotros

    1. Usar CSS-in-JS lib
    2. “Namespace” todo nuestro CSS

#### CSS Proveniente de una librería de componentes o librería de css

    1.  Usar component lib que haga css-in-js
    2.  Build manual de la librería y aplicar técnicas de namespacing en ellas

### IMPLEMENTANDO Multi-Tier Navigation

##### El container y las xapss necesitan feature de ruteo

    1.  Los usuarios pueden navegar por la diferentes xapss usando lógica a de ruteo construida en el container.
    2.  Lo usuarios pueden navegar por las subapps usando lógica a construida en la subapp misma.
    3.  No todas las xapps pueden requerir ruteo

##### Xapps quizas necesiten poder agregar nuevas paginas/rutas todo el tiempo

    1.  Las nuevas rutas creadas no deberían requerir del redeploy del container

##### Quizas necesitemos mostrar dos o mas MF al mismo tiempo

    1.  Esto puede ocurrir todo el tiempo, por ejemplo el caso de un sidebar de carrito sobre la home

##### Necesitamos usar soluciones de ruteo “off-the-shell”

    1.  Crear una librería nueva puede ser muy duro.
    2.  Usar código custom que refuerza lo que ya hay en tal framework de xapps

##### Necesitamos feature de navegación para las xapss en modo host e insolation

    1.  Desarrollar en cada ambiente debería ser fácil, cada desarrollador debería inmediatamente poder ver que path están visitando.

##### Si las diferentes xapss necesitan comunicar información entre ellas sobre ruteo, esta bien que sea de manera mas genérica posible

    1.  Cada xapss quizás este usando una framewokr completamente dif de navegación
    2.  Quinas necesitemos cambiar o hacer un upgrade de las librerías de navegación. Esto no tendría que llevar un proceso de re escritura en el resto de la xapp

##### AUTENTICACION

    1.  Auth app es para signing in/up users
    2.  No es para saber si el user esta logueado o no, no para “enforcing permission” o permisos a ciertas rutas.
    3.  2 apporaches para trabajar auth
        a.  Cada es app se ocupa de la tuh
        b.  Centralzar auth en container

##### COSAS A TENER EN CUENTA

    1.  Los requerimiento manejan la arquitectura
    2.  Siempre preguntarnos “si esto va cambia en el futuro. Tendre que cambiar otra xapo?
    3.  Todos, eventualmente, olvidamos como funciona react
    4.  No olvidarnos del alcance de nuestro css
    5.  MF puede causar issues en prod que no vemos en dev
