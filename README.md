# Procesamientos de archivos

## 1. Comando cat

Modo de uso: 

```
$ cat <opciones> <fichero>
```

### 1.1 Crear un archivo

```bash
cat > file.txt
```

### 1.2 Ver el contenido de un archivo

```bash
cat file.txt
```

### 1.3 Rederigir el contenido

```bash
cat file.txt > destino.txt
```

### 1.4 Concatenar archivos

```bash
cat file1.txt file2.txt > destino.txt
```

### 1.5 Mostrar número de líneas

```bash
cat -n file.txt
```

## 2. Comando cat

Modo de uso: 

```
$ paste <opciones> <fichero>
```

* Fusiona líneas de archivos.
  
* Escribe lineas de texto en la salida estándar. Estás líneas se generan a partir de la fusión de cada una de las líneas de los archivos separados por un tabulador.

Opciones:

`-s`: Fusiona un archivo por vez de forma serial. 

`-d`: Separa la unión de los archivos por la lista de separadores que se especifica. 

### 2.1 Practiquemos:

```
# Crearemos nuestros datos en los archivos samples.txt, genes.txt y expression.txt
$ cat > samples.txt 
kid 
kid 
adult 
adult 
adult

$ cat > genes.txt
SOX13
BAF1
TCF4
PAX5
CTCF

$ cat > expression.txt
up
up
down
down
up


$ paste samples.txt genes.txt expression.txt 
kid SOX13 up
kid BAF1 up
adult TCF4 down
adult PAX5 down
adult CTCF up

$ paste -s samples.txt genes.txt expression.txt 
kid kid adult adult adult
SOX13 BAF1 TCF4 PAX5 CTCF
up up down down up


$ paste -d ":" samples.txt genes.txt expression.txt 
kid:SOX13:up
kid:BAF1:up
adult:TCF4:down
adult:PAX5:down
adult:CTCF:up

$ paste -d ":," samples.txt genes.txt expression.txt 
kid:SOX13,up
kid:BAF1,up
adult:TCF4,down
adult:PAX5,down
adult:CTCF,up
```

## 3. Comando join

Modo de uso: 

```
$ join <opciones> <fichero1> <fichero2>
```

### 3.1 Practiquemos

```
# Crearemos nuestros datos en los archivos prueba1.txt y prueba2.txt

$ cat > pruea1.txt # La primera columna es el DNI y la segunda columna el nombre.
11321345,Mario Neta
20324151,Aquiles Bailo
12415132,Elsa Blazo
32412512,Elena Nito
13241351,César Noso

$ cat > prueba2.txt # La primera columna es el DNI y la segunda columna el sueldo
11321345,10000
20324151,8700
12415132,10500
32412512,6500
13241351,9000

$ cat > pruea3.txt # La primera columna es el nombre y la segunda columna el DNI.
11321345,Mario Neta
20324151,Aquiles Bailo
12415132,Elsa Blazo
32412512,Elena Nito
13241351,César Noso

$ join -t "," prueba1.txt prueba2.txt
11321345,Mario Neta,10000
20324151,Aquiles Bailo,8700
12415132,Elsa Blazo,10500
32412512,Elena Nito,6500
13241351,César Noso,9000

$ join -1 2 -2 1 -t "," prueba3.txt prueba2.txt
11321345,Mario Neta,10000
20324151,Aquiles Bailo,8700
12415132,Elsa Blazo,10500
32412512,Elena Nito,6500
13241351,César Noso,9000

$ nano pruea1.txt
11321345,Mario Neta
20324151,Aquiles Bailo
32412512,Elena Nito
13241351,César Noso
**12415132,Elsa Blazo**

$ nano prueba2.txt 
11321345,10000
20324151,8700
**12415132,10500**
32412512,6500
13241351,9000

$ join -t "," prueba1.txt prueba2.txt
```
