# Práctica del curso de git, gitHub y Sourcetree
## Alumno: Cristian Blázquez Bustos
### Ejercicio1

#### ¿Qué comando utilizaste en el paso 11? ¿Por qué?
`git reset --hard HEAD~1`

Mediante el comando reset puedo posicionar el puntero **HEAD** y el puntero de rama al commit que yo quiera, en éste caso le indico que se posicione en **HEAD~1** o, lo que es igual, en el padre del commit actual. 
Haciendo uso del modificador **--hard** indico a git que, además de posicionarse en el commit anterior, modifique mi Working Copy para que su contenido sea el del commit al que acabo de posicionarme.


#### ¿Qué comando o comandos utilizaste en el paso 12? ¿Por qué?
En primer lugar, utilizo el comando `git reflog` para acceder al log de operaciones realizadas sobre el repositorio, en busca del commit deshecho:
~~~
ee88b92 (HEAD -> styled, master) HEAD@{0}: reset: moving to HEAD~1
460c9f2 HEAD@{1}: commit: Modificación de archivo git-nuestro.md para darle estilo MarkDown
ee88b92 (HEAD -> styled, master) HEAD@{2}: checkout: moving from master to styled
ee88b92 (HEAD -> styled, master) HEAD@{3}: commit (initial): Creación de archivo git-nuestro.md
~~~
Identificado el commit al que quiero volver (**460c9f2**), ejecuto el comando `git reset --hard 460c9f2` para posicionar HEAD y el puntero de rama al commit deseado, haciendo uso del argumento **--hard** para que el contenido de Working Copy corresponda con los cambios realizados en el commit en el que deseo posicionarme.


#### El merge del paso 13, ¿Causó algún conflicto? ¿Por qué?
No, puesto que la rama master no ha experimentado ningún avance desde la creación de la rama styled (no se ha realizado commit de ningún cambio en la rama master), por tanto no hay ningún cambio que pueda incorporar desde la rama master a la rama styled. Por ello, al ejecutar el comando `git merge master` git devuelve el mensaje:
~~~
"Already up to date."
~~~

#### El merge del paso 19, ¿Causó algún conflicto? ¿Por qué?
Tras cambiarme a la rama styled (`git checkout styled`) y solicitar el merge de la rama htmlify (`git merge htmlify`) git informa que han aparecido conflictos y no puede realizar el merge de manera automática:
~~~
"CONFLICT (content): Merge conflict in git-nuestro.md"
~~~
El motivo es que tanto en la rama styled como en la rama htmlify existen modificaciones del fichero *git-nuestro* **sobre las mismas líneas**, por lo que git no puede decidir automáticamente qué cambios deben prevalecer.

#### El merge del paso 21, ¿Causó algún conflicto? ¿Por qué?
Tras posicionarme en la rama master(`git checkout master`) y ejecutar la operación merge de la rama styled (`git merge styled`) la operación se realiza automáticamente y no aparece ningún conflicto.
El motivo es que la rama styled fue creada a partir de la rama master en un momento en el que el fichero *git-nuestro* ya existía. Dado que en la rama master no se ha realizado ningun cambio sobre el fichero desde la creación de la rama styled, git puede incorporar los cambios de styled sobre la rama master ya que han sido realizados sobre el mismo fichero de base (y son, por tanto, una versión actualizada del mismo).

#### ¿Qué comando o comandos utilizaste en el paso 25?
`git log --graph --decorate --pretty=oneline`

El comando **log** muestra el historial de commits que han sucedido en el repositorio desde la posición a la que apunta HEAD actualmente, mediante el modificador **--graph** le indicamos que muestre el log en forma de grafo, mediante el modificador **--decorate** muestra la salida con colores y el modificador **--pretty=oneline** indica que muestre cada commit en una única línea
~~~
*   9e92e61f4203cc6e3c1980a47e738b3f7d80443c (HEAD -> master, styled) Merge branch 'htmlify' into styled
|\
| * 2aeb0004a8ac710b4f90fd8663bb9b974c3eb6cd (htmlify) Aplicación de estilos html en archivo git-nuestro.md
* | 460c9f292336fbd6cdac52e17a4e471d08383a8a Modificación de archivo git-nuestro.md para darle estilo MarkDown
|/
* ee88b929a5c8a86ad63bbd1eab08d734d19ba0dc Creación de archivo git-nuestro.md
~~~

#### El merge del paso 26, ¿Podría ser fast forward? ¿Por qué?
Sí, ya que la rama master no ha avanzado desde la creación de la rama title por lo que desde la rama title se tiene acceso a todos los commits de la rama master. Al hacer un merge con fast forward el puntero Head de la rama master avanzaría hasta la posición de la rama title sin dejar ningún commit inaccesible.

#### ¿Qué comando o comandos utilizaste en el paso 27?
`git reset HEAD~1`

Como vimos en pasos anteriores, **HEAD~1** hace referencia al padre del commit actual. Aunque a priori el commit actual tiene dos padres (último commit de la rama master y último commit de la rama title), git reconoce como padre principal al commit de la rama que absorbió a title, por lo que puede posicionarse sin problemas.
Al ejecutar el comando git devuelve el siguiente mensaje:
~~~
"Unstaged changes after reset:
M	git-nuestro.md"
~~~
Al no haber utilizado el argumento **--hard** el directorio Working Copy permanece con las mismas modificaciones que existían antes de ejecutar el comando reset, por lo que el fichero *git-nuestro* de Working Copy (al que se había agregado el título al hacer el merge con la rama title) en éste momento contiene cambios respecto al mismo fichero del commit en el que nos encontramos actualmente

#### ¿Qué comando o comandos utilizaste en el paso 28?
`git checkout git-nuestro.md`

#### ¿Qué comando o comandos utilizaste en el paso 29?
`git branch -D title`

#### ¿Qué comando o comandos utilizaste en el paso 30?
En primer lugar ejecuto `git reflog` para buscar el commit correspondiente al merge de la rama master con la rama title:
~~~
9e92e61 (HEAD -> master, styled) HEAD@{0}: reset: moving to HEAD~1
66616df HEAD@{1}: merge title: Merge made by the 'recursive' strategy.
9e92e61 (HEAD -> master, styled) HEAD@{2}: checkout: moving from title to master
8c3f12e HEAD@{3}: commit: Agrego título al archivo git-nuestro.md
9e92e61 (HEAD -> master, styled) HEAD@{4}: checkout: moving from master to title
9e92e61 (HEAD -> master, styled) HEAD@{5}: merge styled: Fast-forward
ee88b92 HEAD@{6}: checkout: moving from styled to master
9e92e61 (HEAD -> master, styled) HEAD@{7}: commit (merge): Merge branch 'htmlify' into styled
460c9f2 HEAD@{8}: checkout: moving from htmlify to styled
2aeb000 (htmlify) HEAD@{9}: commit: Aplicación de estilos html en archivo git-nuestro.md
ee88b92 HEAD@{10}: checkout: moving from master to htmlify
ee88b92 HEAD@{11}: checkout: moving from styled to master
460c9f2 HEAD@{12}: reset: moving to 460c9f2
ee88b92 HEAD@{13}: reset: moving to HEAD~1
460c9f2 HEAD@{14}: commit: Modificación de archivo git-nuestro.md para darle estilo MarkDown
ee88b92 HEAD@{15}: checkout: moving from master to styled
ee88b92 HEAD@{16}: commit (initial): Creación de archivo git-nuestro.md
~~~

Identificado el commit correspondiente al merge (**66616df**), ejecuto el comando `git reset --hard 66616df` para posicionar HEAD y master en el commit correspondiente, haciendo uso del argumento **--hard** para que el contenido de Working Copy corresponda con el estado del commit al que me posiciono.

#### ¿Qué comando o comandos usaste en el paso 32?
En primer lugar, ejecuto `git reflog` para buscar el commit correspondiente a la creación del fichero *readme*:
~~~
66616df (HEAD -> master) HEAD@{0}: checkout: moving from master to master
66616df (HEAD -> master) HEAD@{1}: reset: moving to 66616df
9e92e61 HEAD@{2}: reset: moving to HEAD~1
66616df (HEAD -> master) HEAD@{3}: merge title: Merge made by the 'recursive' strategy.
9e92e61 HEAD@{4}: checkout: moving from title to master
8c3f12e HEAD@{5}: commit: Agrego título al archivo git-nuestro.md
9e92e61 HEAD@{6}: checkout: moving from master to title
9e92e61 HEAD@{7}: merge styled: Fast-forward
ee88b92 HEAD@{8}: checkout: moving from styled to master
9e92e61 HEAD@{9}: commit (merge): Merge branch 'htmlify' into styled
460c9f2 HEAD@{10}: checkout: moving from htmlify to styled
2aeb000 HEAD@{11}: commit: Aplicación de estilos html en archivo git-nuestro.md
ee88b92 HEAD@{12}: checkout: moving from master to htmlify
ee88b92 HEAD@{13}: checkout: moving from styled to master
460c9f2 HEAD@{14}: reset: moving to 460c9f2
ee88b92 HEAD@{15}: reset: moving to HEAD~1
460c9f2 HEAD@{16}: commit: Modificación de archivo git-nuestro.md para darle estilo MarkDown
ee88b92 HEAD@{17}: checkout: moving from master to styled
ee88b92 HEAD@{18}: commit (initial): Creación de archivo git-nuestro.md
~~~

Identificado el commit buscado (**ee88b92**), ejecuto el comando `git reset --hard ee88b92` para posicionar HEAD y master en el commit correspondiente. Utilizo el argumento **--hard** para que el contenido de Working Copy corresponda con el estado del commit al que me posiciono y poder validar que me he posicionado correctamente.

#### ¿Qué comando o comandos usaste en el punto 33?
En primer lugar, ejecuto `git reflog` para buscar el commit correspondiente al estado final (con el título del poema y habiendo realizado el merge de master con title):
~~~
ee88b92 (HEAD -> master) HEAD@{0}: reset: moving to ee88b92
66616df HEAD@{1}: checkout: moving from master to master
66616df HEAD@{2}: reset: moving to 66616df
9e92e61 HEAD@{3}: reset: moving to HEAD~1
66616df HEAD@{4}: merge title: Merge made by the 'recursive' strategy.
9e92e61 HEAD@{5}: checkout: moving from title to master
8c3f12e HEAD@{6}: commit: Agrego título al archivo git-nuestro.md
9e92e61 HEAD@{7}: checkout: moving from master to title
9e92e61 HEAD@{8}: merge styled: Fast-forward
ee88b92 (HEAD -> master) HEAD@{9}: checkout: moving from styled to master
9e92e61 HEAD@{10}: commit (merge): Merge branch 'htmlify' into styled
460c9f2 HEAD@{11}: checkout: moving from htmlify to styled
2aeb000 HEAD@{12}: commit: Aplicación de estilos html en archivo git-nuestro.md
ee88b92 (HEAD -> master) HEAD@{13}: checkout: moving from master to htmlify
ee88b92 (HEAD -> master) HEAD@{14}: checkout: moving from styled to master
460c9f2 HEAD@{15}: reset: moving to 460c9f2
ee88b92 (HEAD -> master) HEAD@{16}: reset: moving to HEAD~1
460c9f2 HEAD@{17}: commit: Modificación de archivo git-nuestro.md para darle estilo MarkDown
ee88b92 (HEAD -> master) HEAD@{18}: checkout: moving from master to styled
ee88b92 (HEAD -> master) HEAD@{19}: commit (initial): Creación de archivo git-nuestro.md
~~~
Identificado el commit buscado (**66616df**), ejecuto el comando `git reset --hard 66616df` para posicionar HEAD y master en el commit correspondiente, haciendo uso del argumento **--hard** para que el contenido de Working Copy corresponda con el estado del commit al que me posiciono.
