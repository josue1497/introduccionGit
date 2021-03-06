# RAMIFICACIONES

La ramificación es uno de los mecanismos más interesantes y usados en los sistemas de control de versiones modernos, al hablar de ramificaciones, hablamos que hemos tomado la rama principal de desarrollo (master) y a partir de ella hemos continuado trabajando sin seguir la rama principal de desarrollo, es decir en una rama diferente.

Un ejemplo de esto podría ser el desarrollo del sistema operativo universal GNU/debian, el equipo de debian mantiene siempre tres versiones de su sistema, el estable, en pruebas y de desarrollo (actualmente estas ramas son wheezy, jessie y sid) . La __rama estable__ es donde los paquetes han sido probados y están listos para salir al mundo con un minino o nulos bugs, en la __rama en pruebas__ sin embargo es donde se mantienen los paquetes más actualizados pero que aun no ha superado la fase de pruebas para salir al mundo e integrarse a la rama estable, mientras que la __rama de desarrollo__ están los paquetes, módulos del sistema, etc. que aun no están listos para producción y que están cambiando constantemente y que pueden terminar agregándose o ser eliminados en cualquier momento. 

Procedamos entonces a crear nuestra primera ramificación, actualmente ya estamos trabajando bajo una rama y es la rama master que por defecto git nos crea y que es donde hemos estado realizando los cambios, así que crearemos una rama nueva, cabe mencionar que al crear una rama a partir de otra, la nueva rama contiene, inicialmente, los mismos archivos y commits que la rama base, hasta que modifiquemos o agreguemos nuevos archivos y directorios.

```
$ git branch testing 
$ git status
# On branch master 
nothing to commit (working directory clean)
```

Al parecer no hemos hecho nada, pero hemos creado una rama nueva llamada testing, para ver que esto es cierto ejecutemos:

```
$ git branch
* master 
  testing
```
  
El __'*'__ dos dice en que rama estamos actualmente, cambiemos de rama entonces con git checkout:

```
$ git checkout testing 
Switched to branch 'testing' 
$ git status 
# On branch testing 
nothing to commit (working directory clean)
```

Vamos a agregar un par de lineas de código a un archivo

```
$ cat script2.py 
#! /usr/bin/env python 
for a in [1, 2]: 
    for b in ["a", "b"]: 
        print a, b
```
```
$ git status
# On branch testing 
# Changes not staged for commit: 
#   (use "git add <file>..." to update what will be committed) 
#   (use "git checkout -- <file>..." to discard changes in working directory) 
# 
#	modified:   script2.py 
# 
no changes added to commit (use "git add" and/or "git commit -a") 
```
```
$ git add . 
$ git commit -am "combinaciones en python" 
[testing d3dbf28] combinaciones en python 
 1 file changed, 5 insertions(+) 
 mode change 100644 => 100755 script2.py 
```

Añadamos un nuevo archivo para luego unir las ramas testing y master en una sola:

```
$ touch script3.py 
$ git add . 
$ git commit -am "3er script" 
[testing 0294943] 3er script 
 0 files changed 
 create mode 100644 script3.py 
$ git status 
# On branch testing 
nothing to commit (working directory clean)
```

hagamos un simple log de los 3 últimos commits hechos en la rama testing:

```
$ git log -3 --pretty=short 
commit 0294943eb590210846c2875a07b5a24b08360c52 
Author: Mr. Floyd <mr.floyd@cshluesocc.org>

    3er script 

commit d3dbf28039a7fede95eafea4c0f5378c144c34d6 
Author: Mr. Floyd <mr.floyd@cshluesocc.org>

    combinaciones en python 

commit f3e208303b789e5d944f11a6565f1514714f4132 
Author: Mr. Floyd <mr.floyd@cshluesocc.org>

    script.py renombrado a holaMundo.py 
```

Regresemos al la rama master y hagamos también un log:

```
$ git checkout master 
Switched to branch 'master' 
$ git log -3 --pretty=short 
commit f3e208303b789e5d944f11a6565f1514714f4132 
Author: Mr. Floyd <mr.floyd@cshluesocc.org>

    script.py renombrado a holaMundo.py 

commit 0f6d43a9c0687fa70ca95700aae45d75b782bb43 
Author: Mr. Floyd <mr.floyd@cshluesocc.org>

    oops borrado correctamente 

commit 43af71fcd7a71e468ebfa57d059f51bd5a9cacee 
Author: Mr. Floyd <mr.floyd@cshluesocc.org>

    commit accidental 
```

en nuestra rama master no están los últimos commits que hicimos en la rama testing, tenemos que unir las ramas para que los cambios en testing se apliquen a la rama master, para eso usaremos el comando git merge así:

```
$ git merge testing 
Updating f3e2083..0294943 
Fast-forward 
 script2.py |    5 +++++ 
 1 file changed, 5 insertions(+) 
 mode change 100644 => 100755 script2.py 
 create mode 100644 script3.py 
```
```
$ git status 
# On branch master 
nothing to commit (working directory clean) 
$ ls 
holaMundo.py  README  script2.py  script3.py 
$ cat script2.py 
#! /usr/bin/env python 

for a in [1, 2]: 
    for b in ["a", "b"]: 
        print a, b 
```

Vemos que tenemos los archivos actualizados según como los teníamos en la rama testing, hagamos un pequeño log para ver que ha pasado:

```
$ git log -4 --pretty=short 
commit 0294943eb590210846c2875a07b5a24b08360c52 
Author: Mr. Floyd <mr.floyd@cshluesocc.org>

    3er script 

commit d3dbf28039a7fede95eafea4c0f5378c144c34d6 
Author: Mr. Floyd <mr.floyd@cshluesocc.org>

    combinaciones en python 

commit f3e208303b789e5d944f11a6565f1514714f4132 
Author: Mr. Floyd <mr.floyd@cshluesocc.org> 

    script.py renombrado a holaMundo.py 

commit 0f6d43a9c0687fa70ca95700aae45d75b782bb43 
Author: Mr. Floyd <mr.floyd@cshluesocc.org>

    oops borrado correctamente 
```

Tenemos todo actualizado.

__Un par de observaciones:__  si editamos/agregamos/borramos algo en un a rama y no hacemos los respectivos adds, commits y nos cambiamos a una rama distinta, esta acción pueda que nos genere problemas en la rama a la que nos movimos pues esos cambios también estarán en esta ultima a pesar que no hayamos trabajado en ella. Por lo anterior se recomienda hacer los respectivos commit en la rama actual (dejando limpio el repositorio) antes de moverse a una rama distinta.

_«Si crees que falta algo que aclarar o añadir en esta parte, por favor ayúdanos a mejorarla...»_

Como ya hemos unidos nuestros cambios de la rama testing en la rama master, hagamos de cuenta que ya no necesitamos la rama testing y queremos borrarla. Para borrar una rama usamos __git branch__ con la opción __-d__ como en el siguiente ejemplo:

```
$ git branch -d testing
Deleted branch testing (was 060ac2d).
$ git branch 
* master 
```

A groso modo esto es la base de las ramificaciones en git, puedes visitar la documentación oficial para más info y algunas buenas practicas. También este enlace puede ser útil http://nvie.com/posts/a-successful-git-branching-model/ en el encontraras un par de consejos para crear un modelo de ramas exitoso para tu trabajo en git en equipo o bien proyectos personales.

_«Si crees conveniente y deseas ampliar esta sección no dudes en hacerlo, toda ayuda es bienvenida :)»_
  
  
  
  
  