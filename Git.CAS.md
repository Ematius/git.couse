      - [Características de Git como SCV distribuido](#características-de-git-como-scv-distribuido)
        - [PowerShell](#powershell)
        - [Git commit. Mensajes de commit](#git-commit-mensajes-de-commit)
        - [git log / git show](#git-log--git-show)
      - [Referencias relativas](#referencias-relativas)
        - [VSCode](#vscode)
      - [Comandos de conexión y uso del repositorio remoto (resumen)](#comandos-de-conexión-y-uso-del-repositorio-remoto-resumen)
      - [La carpeta .git](#la-carpeta-git)
      - [Hashes: creación y lectura](#hashes-creación-y-lectura)
      - [Elementos de un repositorio- Primer commit](#elementos-de-un-repositorio--primer-commit)
      - [Modificación de un archivo](#modificación-de-un-archivo)
      - [References: Branches and HEAD](#references-branches-and-head)
    - [Taller: Crear un nueva repositorio desde cero](#taller-crear-un-nueva-repositorio-desde-cero)
      - [Gestionando el repositorio con comandos plumbing](#gestionando-el-repositorio-con-comandos-plumbing)
      - [Creando un commit con comandos plumbing](#creando-un-commit-con-comandos-plumbing)
      - [En resumen del Taller](#en-resumen-del-taller)
        - [Actualizaciones de las ramas feature](#actualizaciones-de-las-ramas-feature)
**Alejandro Cerezo Lasne** <alce65@hotmail.es>
![SVC centralizado](assets/svc-centralized.png)
![SVC distribuido](assets/svc-distributed.png)
#### Características de Git como SCV distribuido
- la cache de ficheros y la posibilidad de incluir enlaces simbólicos (desactivada por defecto)
##### PowerShell

El primer paso es habilitar la ejecución de scripts en PowerShell con el comando, adecuado, según se trate de hacerlo para el usuario actual o para todos los usuarios del sistema.

```shell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
Set-ExecutionPolicy -Scope LocalMachine -ExecutionPolicy RemoteSigned -Force
```

El siguiente paso es instalar el módulo de posh-git con el comando

```shell
Install-Module posh-git -Scope CurrentUser -Force
```

Finalmente hay que añadir la importación del módulo en el fichero de configuración de PowerShell, que se encuentra en el directorio de usuario en el fichero `Microsoft.PowerShell_profile.ps1`. Para ello, se pueden utilizar los comandos

```shell
Import-Module <path-to-uncompress-folder>\src\posh-git.psd1
Add-PoshGitToProfile -AllHosts
```

![Areas de un repositorio Git](assets/git-areas.svg)

**Git add** es el comando que se utiliza para añadir ficheros al área de preparación. Se puede utilizar de varias formas

**Git status** nos muestra el estado de los ficheros en el directorio de trabajo y en el área de preparación.
##### Git commit. Mensajes de commit

**Git commit** es el comando que se utiliza para guardar en el repositorio los cambios que previamente se han añadido al área de preparación con `git add`.

```shell
git commit -m "Mensaje del commit"
```

Un commit es una **instantánea** (**snapshot**) de los cambios en el repositorio. En el se guardan los ficheros nuevos o modificados que se han añadido al área de preparación. Cada commit tiene una serie de elementos

- un SHA (Secure Hash Algorithm) que es un identificador único
- un autor
- un mensaje que describe los cambios realizados
- una fecha y hora de creación
- una referencia al commit padre (excepto el primer commit)
- un árbol cambios en los archivos y directorios

Más adelante veremos en que consisten exactamente los commits desde un punto de vista técnico, y como se combinan con otros elementos de git, **blobs** y **trees**, para construir el repositorio.
Los **mensajes de commit** son una parte muy importante de Git. Un buen mensaje de commit debe
- solo la primera palabra en mayúsculas
- comienza con un verbo en imperativo
- no se termina el mensaje con un punto
- no ser más largo de 50 (70) caracteres
- no ser un resumen de los cambios
- ser conciso y descriptivo
- contar su qué y sobre todo por qué, cómo
- reflejar la granularidad del commit
##### git log / git show

**Git log** es el comando que se utiliza para ver el historial de commits de un repositorio. Muestra los commits en orden cronológico inverso, es decir, el último commit en la parte superior.

```shell
git log
```

Es un comando con gran número de opciones que permiten personalizar la salida del comando. Por ejemplo, `git log --oneline` muestra los commits en una sola línea. Más adelante volveremos a hablar de este comando.

**Git show** es el comando que se utiliza para ver los cambios de un commit. Muestra los cambios realizados en un commit en relación con su padre.

```shell
git show
```
- Comando y opciones
- Relativas: desde cualquiera de las anteriores, se puede acceder a commits anteriores o posteriores
#### Referencias relativas

Las referencias relativas permiten acceder a commits anteriores o posteriores a un commit dado.

Utilizan los siguientes operadores

- ~ (tilde): seguido de un número, indica el commit al que acceder
- ^ (circunflejo): el padre

Por ejemplo

- `HEAD~1`: El padre del commit actual
- `HEAD~n`: El n-ésimo padre del commit actual
- `HEAD^`: El padre del commit actual
- `HEAD^^`: El abuelo del commit actual
##### VSCode

VSC (Visual Studio Code) incluye soporte nativo para Git, permitiendo realizar las operaciones más comunes desde el propio editor.

Además existen numerosos plugins que amplían las funcionalidades de Git en VSC

- Git Graph
- GitLens
- Git History
Con el fin de hacer disponible los repositorios de Git en la red, existen en Git 2 tipos de repositorios
- **Bare** (desnudo, simple): no tiene directorio de trabajo. No se usa para el directamente en el desarrollo de aplicaciones ya que en él no se pueden realizar directamente commits. De este tipo son los repositorios que se publican, en servidores estándar o especializados en el alojamiento (hosting) de repositorios
- **Desarrollo**: repositorio típico. Mantiene la rama en la que se trabaja, proporciona una copia extraída de dicha rama en el **directorio de trabajo**

El primero de estos formatos se utiliza para publicar los repositorios en servidores de Git públicos, como **Github**, **GitLab** o **Bitbucket** o pertenecientes a una determinada empresa.

El establecimiento de la relación entre el repositorio local y el remoto se puede realizar en cualquiera de las dos direcciones:

- Desde el repositorio local al remoto, mediante el comando `git remote add`

```shell
git remote add origin <url>
git push -u origin main
```

- Desde el repositorio remoto al local, mediante el comando `git clone`

```shell
git clone <url>
```

![Relaciones entre repositorios local y remoto](assets/local_n_remote.svg)

Cualquiera que sea la forma en que se han creado, una vez que existe un remote, los dos operaciones principales en relación xcon el son
#### Comandos de conexión y uso del repositorio remoto (resumen)
- `git clone <url>`
- `git remote add origin https://github.com/alce65/nombre_del_repo.git`
- `git push -u origin main`
- `git push`
- `git pull`
Para conocer/manipular la estructura interna de Git, se utilizan los **plumbing commands** (comandos de fontanería).
Estos comandos son los que utiliza el propio Git internamente para construir los comandos de más alto nivel conocidos como **porcelain commands**.

#### La carpeta .git

En términos de sistema de ficheros, un repositorio Git es una carpeta que incluye un directorio oculto `.git`.
Éste contiene toda la información necesaria para el control de versiones:

- **Objetos**: blobs, trees, commits, tags anotadosls
- **Referencias**: HEAD, master branch, tags ligeros
- Hooks: scripts que se ejecutan en determinados eventos
- Info y Logs: información extra sobre el repo y los commits
- fichero Index: área de preparación
- fichero de configuración local
Se puede ver que el resultado muestra como la carpeta objects consta de una serie de cartetas/archivos que contienen los objetos de Git, por el momento.

- blobs (los archivo)
- trees (los directorios)
- commits (los snapshots)

Más adelante veremos un cuarto tipo de objeto, los tags (anotadas).

#### Hashes: creación y lectura

La siguiente descripción refleja la forma de explicar estos conceptor propuesta por _Paolo Perrotta_ en su conferencia _Understanding Git_, 3 diciembre 2016, disponible en [YouTube](https://www.youtube.com/watch?v=nHkLxts9Mu4).

En su capa más profunda Git es un **mapa de objetos** de acuerdo con un patrón clave/valor. Cada objeto es un cierto contenido (valor) que tiene un hash que lo identifica (clave).

Existe un comando de git que permite obtener el hash de un objeto, `git hash-object`. Este comando toma un archivo o cualquier contenido y devuelve el hash del objeto creado.

```shell
echo "Hello, World" | git hash-object --stdin
// 110fdc0ce1c7582e08f31e17bbcfdec1b50a478c
```

En este comando, el hash para un mismo contenido y en el mismo shell siempre será el mismo. El shell influye porque en cada uno de ellos el stdin es distinto.

El mismo contenido de un archivo siempre tendrá el mismo hash de 40 bytes, sin importar el nombre del archivo o la ubicación en el sistema de archivos. Este hash es único para el contenido con una probabilidad tan alta que se puede considerar único.

> Un hash SHA-1 es un número hexadecimal de 40 caracteres que se utiliza para identificar de forma única los objetos de Git. Se calcula a partir del contenido del objeto y de su tipo.

Si añadimos al comando la opción `-w`, para guardar el objeto obtendremos un mensaje de error porque no nos encontramos en un repositorio de Git.

```shell
echo "Hello, World" | git hash-object -w --stdin
// fatal: not a git repository (or any of the parent directories): .git
```

El mapa de hashes creado por Git se convierte en **persistente** gracias a la existencia de un **repositorio**, que es básicamente un directorio `.git/objects` que contiene todos los objetos de git. Cada objeto es guardado en un archivo con el nombre del hash del objeto.
Creamos un repositorio vacío y guardamos el objeto en el repositorio.

```shell
git init sample-repo
cd sample-repo
dir /a:hd // [= ls -a]
echo "Hello, World" | git hash-object -w --stdin //110fdc0ce1c7582e08f31e17bbcfdec1b50a478c
cd git
dir
cd objects
dir
cd 11
dir // 0fdc0ce1c7582e08f31e17bbcfdec1b50a478c
```

> El repositorio es una **base de datos de objetos** que se almacena en la carpeta oculta `.git`, que contiene los objetos de git en la carpeta objects.
> Por **cada hash** se crea una carpeta con los primeros dos caracteres del hash y dentro de esta carpeta se crea un fichero cuyo nombre es el resto del hash, que contiene la información correspondiente a ese hash en un formato comprimido.

De cada objeto guardado en un archivo se puede recuperar la información que contiene a partir del hash del objeto.

```shell
git cat-file -t 110fdc0 // blob
git cat-file -p 110fdc0 // Hello, World
```
Cada objeto es accesible por los 5 primeros caracteres de su hash:
#### Elementos de un repositorio- Primer commit

Creamos un proyecto muy simple.

```shell
cd cook.book
```

C:.
│ menu.txt // Apple Pie
└───recipes
apple_pie.txt
readme.txt

```shell
cd cook.book
type menu.txt
// Apple Pie
type .\recipes\readme.txt
// Put your recipes in this folder, one for file.
type .\recipes\apple_pie.txt
//Apple Pie
```

Creamos un **repositorio** en el proyecto con un **commit inicial**.

```shell
cd cook.book
git init
git status
git add .
git commit -m "Initial commit"
```

Vemos el resultado en la carpeta objects del repositorio.

```shell
cd .git
cd dir /w // [25]   [5d]   [77]   [8d]   [9e]
cd 1f
```

Comprobamos los **logs** del repositorio

```shell
git log

commit 5da6f7b4682638317b18a5fe0f9edca98aeb1f7c (HEAD -> main)
Author: Alejandro Cerezo <alce65@hotmail.es>
Date:   Sat Sep 28 13:14:18 2024 +0200

    Initial commit
```

El **commit** corresponde al objeto 5d-a6f7b4682638317b18a5fe0f9edca98aeb1f7c que podemos leer con `git cat-file`.

```shell
git cat-file -p 5da6f7b

tree 8de329e56d2bf59ad7ce6df33df79e91a2a4a5a8
author Alejandro Cerezo <alce65@hotmail.es> 1727522058 +0200
committer Alejandro Cerezo <alce65@hotmail.es> 1727522058 +0200

Initial commit
```

Un resultado similar se consigue con el comando `git show --pretty=raw`, aunque en este caso se añade la información del diff.

```shell
git show 5da6f7b

commit 5da6f7b4682638317b18a5fe0f9edca98aeb1f7c
tree 8de329e56d2bf59ad7ce6df33df79e91a2a4a5a8
author Alejandro Cerezo <alce65@hotmail.es> 1727522058 +0200
committer Alejandro Cerezo <alce65@hotmail.es> 1727522058 +0200

    Initial commit

diff --git a/menu.txt b/menu.txt
new file mode 100644
```

Un commit no es mas que un texto, que como cualquier otro objeto de git, tiene un hash que lo identifica. En este texto se incluye la metadata del commit, como el autor, el committer, el mensaje al crearlo y la referencia uno o varios hashes.

En este caso, el commit apunta a un objeto de tipo tree que podemos leer con `git cat-file`.

```shell
git cat-file -p 8de329e

100644 blob 9eed377bbdeb4aa5d14f8df9cd50fed042f41023    menu.txt
040000 tree 25036a158dfdf583f672c11ef79f45c6b0e6347a    recipes
```

El **tree** guarda los nombres de los archivos y directorios (otros tree) que incluye, junto con sus hashes. En este caso, el tree apunta a dos objetos, uno de tipo blob y otro de tipo tree, que nuevamente podemos leer con `git cat-file`.

```shell
git cat-file -p 9eed377

Apple Pie
```

```shell
git cat-file -p 25036a1

100644 blob 9eed377bbdeb4aa5d14f8df9cd50fed042f41023    apple_pie.txt
100644 blob 7708256b70bf5e956ea609a785911a31fc14929f    readme.txt
```

En el caso del tree, la misma información se puede obtener con `git ls-tree`.

```shell
git ls-tree 8de329e

100644 blob 9eed377bbdeb4aa5d14f8df9cd50fed042f41023    apple_pie.txt
100644 blob 7708256b70bf5e956ea609a785911a31fc14929f    readme.txt
```

El objeto tree `25036a1` apunta a dos objetos de tipo blob que . Uno de ellos es nuevamente 9eed377, porque el contenido de `apple_pie.txt` es el mismo que el de `menu.txt`. El otro objeto blob lo podemos leer con `git cat-file`.

```shell
git cat-file -p 7708256

Put your recipes in this folder, one for file.
```

![Primer commit](assets/commit1.svg)

#### Modificación de un archivo

Añadimos otra línea en el archivo `menu.txt` y comprobamos los cambios.

```shell
cd ..\..\..\cook.book
echo Cheesecake>> menu.txt
type menu.txt
```

Creamos un nuevo commit para incorporar los cambios.

```shell
git add .
git commit -m "Add Cheesecake to menu"
git log

commit f1dff43f97543e83cab3f52f054cdcf9b26e8d55 (HEAD -> main)
Author: Alejandro Cerezo <alce65@hotmail.es>
Date:   Sat Sep 28 14:03:08 2024 +0200

    Add Cheesecake to menu
```

De nuevo, el commit es un objeto de tipo que podemos leer con `git cat-file`.

```shell
git cat-file -p f1dff4

tree 972e64a00a72e413bd158352ab3e6e98461bfbea
parent 5da6f7b4682638317b18a5fe0f9edca98aeb1f7c
author Alejandro Cerezo <alce65@hotmail.es> 1727524988 +0200
committer Alejandro Cerezo <alce65@hotmail.es> 1727524988 +0200

Add Cheesecake to menu
```

El commit apunta a un objeto de tipo commit, el commit anterior y a un nuevo objeto de tipo tree que podemos leer con `git cat-file`.

```shell
git cat-file -p 972e64a

100644 blob 48459e5685c4561d0fa6e26a7371041e982c0ff4    menu.txt
040000 tree 25036a158dfdf583f672c11ef79f45c6b0e6347a    recipes
```

El tree sigue siendo el mismo, pero el objeto blob `48459e5` es diferente al anterior.
Si lo leemos con `git cat-file` vemos que contiene la nueva línea.

git cat-file -p 48459e5

Apple Pie
Cheesecake
![Primer commit](assets/commit2.svg)

Utilizando un commit como punto de entrada es posible recorrer todos los objetos de un repositorio de git correspondientes al momento en que se creó el commit, reconstruyendo a partir de ellos el estado del proyecto en ese momento, lo que denominamos una **snapshot del proyecto**.

Cuando un fichero no cambia, git no guarda una nueva versión del fichero, sino que guarda una referencia al fichero anterior. De esta forma, git ahorra espacio en disco y tiempo de ejecución.
En resumen, un commit es un objeto que apunta a un objeto de tipo tree que apunta a uno o varios objetos de tipo blob. Un objeto de tipo blob es un archivo, un objeto de tipo tree es un directorio y un objeto de tipo commit es un snapshot del proyecto en un momento dado.

> Técnicamente, un repositorio es un **grafo dirigido acíclico** donde los nodos son objetos de git (de tipo commit, tree o blob) y las aristas son referencias entre ellos. Con ello se consigue un **filesystem** de alto nivel, inmutable, eficiente y seguro, por encima del filesystem del sistema operativo. Además, gracias a los commits, se convierte en un **version filesystem**, con **control de versiones**.

#### References: Branches and HEAD

Las **ramas** en git son simplemente **punteros a commits**. En detalle son archivos de texto plano almacenados en la carpeta `.git/refs/heads` que contienen el hash del commit al que apuntan.

```shell
dir .git\refs\heads /b

main
```

```shell
type .git\refs\heads\main

f1dff43f97543e83cab3f52f054cdcf9b26e8d55
```

**Crear una nueva rama** se reduce a crear un nuevo archivo en la carpeta `.git/refs/heads` con el nombre de la rama y como contenido el hash del commit un commit, que de momento será el mismo que en la rama anterior.

```shell
git branch feature
dir .git\refs\heads /b

feature
main
```

```shell
type .git\refs\heads\feature

f1dff43f97543e83cab3f52f054cdcf9b26e8d55
```

Si en el SO creamos un fichero con el nombre de la rama y el contenido del hash del commit, git reconocerá la rama.

```shell
echo f1dff43f97543e83cab3f52f054cdcf9b26e8d55 > .git\refs\heads\bad-way
git branch

bad-way
feature
* main
```

```shell
del .git\refs\heads\bad-way

git branch

feature
* main
```

La rama main aparece indicada con un asterisco porque es la **rama actual** (current branch). Eso es lo que indica el **puntero `HEAD`**

El puntero `HEAD` es un archivo de texto plano almacenado en la carpeta `.git` que contiene el nombre de la rama actual. Como se ve en el contenido del archivo, es una referencia a una referencia
ref: refs/heads/main
```

En realidad por cada rama se crea una referencia simbólica en la carpeta `.git/refs/heads` que apunta al hash del commit al que apunta la rama.

```shell
dir .git\refs\heads /b

feature
main
```

```shell
type .git\refs\heads\main

f1dff43f97543e83cab3f52f054cdcf9b26e8d55
```

Como veremos más adelante, algunos de los comandos más importasntes de Git son aquellos que permiten **mover el puntero `HEAD`** y **cambiar de rama**.

### Taller: Crear un nueva repositorio desde cero

En su libro _Gitting Things Done_, 2021, disponible en [Amazon](https://www.amazon.com/-/es/Omer-Rosenbaum/dp/1801070004), _Omer Rosenbaum_ propone un interesante ejercicio destinado a comprender a fondo en que consiste un repositorio deGit, consistente en crearlo desde cero y añadirle un primer commit.

Creamos una nueva carpeta y accedemos a ella.

```shell
mkdir scratch-repo
cd scratch-repo
```

Como es previsible git status nos indica que no estamos en un repositorio de git.

```shell
git status
// fatal: not a git repository (or any of the parent directories): .git
```

Creamos la estructura de carpetas básica de un repositorio.

```shell
mkdir -p .git
cd .git
mkdir objects
mkdir refs
cd refs
mkdir heads
cd..
cd..
tree
```

Obtenemos la siguiente estructura y volvemos a comprobar el estado del repositorio.

```shell
C:.
└───.git
    ├───objects
    └───refs
        └───heads
```

```shell
git status
fatal: not a git repository (or any of the parent directories): .git
```

El paso que nos falta es crear el fichero `HEAD` en la carpeta `.git` que apunte a la rama `main`.

```shell
echo ref: refs/heads/main > .git\HEAD
```

En este punto, git ya reconoce la carpeta como un repositorio.

```shell
git status
// On branch main
// No commits yet
// nothing to commit (create/copy files and use "git add" to track)
```

A pesar de que no existe la rama main en la carpeta `refs/heads/main`, git reconoce la rama `main` como la rama actual porque la tiene referenciada en el fichero HEAD.

#### Gestionando el repositorio con comandos plumbing

Hemos creado el repositorio simplemente con los comandos del SO, sin utilizar `git init`. Vamos a añadir un fichero al área de preparación y hacer el primer commit, pero sin utilizar los comandos porcelain, `git add` o `git commit`. En su lugar utilizaremos los comandos plumbing que subyacen a los de más alto nivel que utilizamos habitualmente.

Para crear un objeto blob, utilizamos el comando `git hash-object`, tal y como ya hemos visto, pasándole información desde el stdin.

```shell
echo Aprendiendo Git | git hash-object --stdin
// 7b31213ab333bd7eab40ce5de1185bd6565f120e
```

También sabemos que el modificados -w guarda el objeto en el repositorio.

```shell
echo Aprendiendo Git | git hash-object -w --stdin
tree .git
dir .git\objects\7b
```

```shell
C:.
└───.git
    ├───objects
    │   └───7b // 31213ab333bd7eab40ce5de1185bd6565f120e
    └───refs
        └───heads
Se ha creado un objeto blob en la carpeta `objects` del repositorio, con una sub-carpeta `7b` y un fichero con el nombre del resto del hash del objeto.
El comando `git cat-file` nos permite leer el tipo y el contenido del objeto.

```shell
git cat-file -t 7b31213 // blob
git cat-file -p 7b31213 // Aprendiendo Git
```

A pesar de ello, vemos que git status no refleja esta cambio

```shell
git status
```

Para añadir el objeto al área de preparación, necesitamos crear el indice, el fichero que contiene información sobre los hashes de los objetos que se han añadido al área de preparación. Para ello utilizaremos el comando `git update-index`.

```shell
git update-index --add --cacheinfo 100644 7b31213ab333bd7eab40ce5de1185bd6565f120e README.md
```

Indicamos la mascara del fichero a nivel del SO (100644), el hash del objeto y el nombre del fichero, que aun no existe.

EL resultado a nivel de git, es

```shell
git status

// Changes to be committed: new file:   README.md
// Changes not staged for commit: deleted:    README.md
```

El fichero está en el area de preparación, pero no en el directorio de trabajo. Para añadirlo al directorio de trabajo, crearemos el fichero con el mismo contenido que el objeto blob.

```shell
echo Aprendiendo Git > README.md
```

Y comprobamos que el fichero ha sido añadido al directorio de trabajo y aparece como staged en el área de preparación, seg´´un nos indica `git status`.

```shell
git status

// Changes to be committed: new file:   README.md
```

#### Creando un commit con comandos plumbing

El paso de la información desde la staged area al repositorio exige que creemos un objeto tree que contenga la información de los objetos que se han añadido al área de preparación. Para ello utilizamos el comando `git write-tree`. Para comprobar el contenido del objeto tree, utilizamos el comando `git cat-file`.

```shell
git write-tree // 3976f2ec82f4d61250cac1b1b26fa053439dbcae
git cat-file -t 3976f2e // tree
git cat-file -p 3976f2e // 100644 blob 7b31213ab333bd7eab40ce5de1185bd6565f120e    README.md
```

A nivel de carpetas, veremos que se a creado un objeto tree en la carpeta `objects` del repositorio.

```shell
tree .git
dir .git\objects\39
```

```shell
C:.
└───.git
    ├───objects
    │   └───39 // 76f2ec82f4d61250cac1b1b26fa053439dbcae
    │   └───7b // 31213ab333bd7eab40ce5de1185bd6565f120e
    └───refs
        └───heads
```

A partir del objeto tree, creamos un commit con el comando `git commit-tree`. Para ello necesitamos el hash del objeto tree, que ya conocemos.

```shell
git commit-tree 3976f2ec82f4d61250cac1b1b26fa053439dbcae -m "Initial commit" // 79aa70c84454bd9e928f0224139170837c8563c8
```

Nuestro objeto commit puede comprobarse con `git cat-file`.

```shell
git cat-file -t 79aa70c // commit
git cat-file -p 79aa70c // tree 3976f2ec82f4d61250cac1b1b26fa053439dbcae
```

Y nuestro repositorio refleja su existencia a nivel de carpetas pero no en el resultado de `git status`.

```shell
tree .git
dir .git\objects\79
```

```shell
C:.
└───.git
    ├───objects
    │   └───39 // 76f2ec82f4d61250cac1b1b26fa053439dbcae
    │   └───79 // aa70c84454bd9e928f0224139170837c8563c8
    │   └───7b // 31213ab333bd7eab40ce5de1185bd6565f120e
    └───refs
        └───heads
```

```shell
git status
// Changes to be committed:  new file:   README.md
```

El problema es simplemente que no se ha actualizado el puntero de la rama `main` al nuevo commit. En realidad, el fichero refs/heads/main al que hace referencia HEAD ni siquiera existe. Debemos crearlo con el hash del nuevo commit.

```shell
echo 79aa70c84454bd9e928f0224139170837c8563c8 > .git\refs\heads\main
```

De esta forma tanto `git status` como `git log` reflejarán la existencia del nuevo commit.

```shell
git status
// nothing to commit, working tree clean
git log
// commit 79aa70c84454bd9e928f0224139170837c8563c8 (HEAD -> main)
```

#### En resumen del Taller

Crear un nueva repositorio desde cero ha sido posible gracias a los comandos del SO y a los comandos de bajo nivel de Git, los **plumbing commands**.

Hemos creado un objeto blob, un objeto tree y un objeto commit, y hemos actualizado los punteros de la rama y HEAD para reflejar la existencia del nuevo commit. Pare ello hemos usado los siguientes comandos:

- `git hash-object`: para crear un objeto blob
- `git update-index`: para añadir el objeto al área de preparación
- `git write-tree`: para crear un objeto tree
- `git commit-tree`: para crear un objeto commit
- `git cat-file`: para leer el contenido de los objetos

Los alias de git admiten los mismos parámetros y modificadores que los comandos originales

```shell
git config --global alias.ch git checkout
git ch -b feature/branch
```

- GitHub Flow, Feature Branching, Trunk Based Development