# Procesamiento de archivos

El procesamiento de archivos es una parte esencial de los análisis bioinformáticos, ya que permite manipular y visualizar datos de secuencias, anotaciones genómicas y otros tipos de datos biológicos. A continuación se detallan algunos comandos y su aplicación en bioinformática.

## 1. Comando cat

El comando cat (concatenate) se utiliza para crear, visualizar y concatenar archivos de texto. En bioinformática, es común trabajar con archivos de secuencias en formato FASTA o FASTQ, archivos de anotaciones en formato GFF o BED, entre otros.

Modo de uso: 

```
$ cat <opciones> <fichero>
```

### 1.1 Crear un archivo

Para crear un archivo, puedes redirigir la salida estándar al archivo deseado. Por ejemplo, para crear un archivo de secuencia:

```bash
$ cat > seq1.fasta
```

Luego puedes escribir la secuencia en formato FASTA y presionar Ctrl+D para guardar el archivo.

### 1.2 Ver el contenido de un archivo

Para visualizar el contenido de un archivo, como un archivo FASTA que contiene secuencias de ADN:

```bash
$ cat seq1.fasta
```

### 1.3 Rederigir el contenido

Puedes redirigir el contenido de un archivo a otro. Esto es útil cuando deseas crear una copia de un archivo de anotaciones genómicas:

```bash
$ cat > annotations.gff
##gff-version 3
chr1  .  gene          1300  9000  .  +  .  ID=gene00001;Name=GeneA
chr1  .  mRNA          1300  9000  .  +  .  ID=mRNA00001;Parent=gene00001;Name=GeneA-RA
chr1  .  exon          1300  1500  .  +  .  ID=exon00001;Parent=mRNA00001
chr1  .  exon          3000  3902  .  +  .  ID=exon00002;Parent=mRNA00001
chr1  .  CDS           1300  1500  .  +  0  ID=cds00001;Parent=mRNA00001
chr1  .  CDS           3000  3902  .  +  0  ID=cds00002;Parent=mRNA00001
chr1  .  gene          10500 13500 .  -  .  ID=gene00002;Name=GeneB
chr1  .  mRNA          10500 13500 .  -  .  ID=mRNA00002;Parent=gene00002;Name=GeneB-RA
chr1  .  exon          10500 10700 .  -  .  ID=exon00003;Parent=mRNA00002
chr1  .  exon          12000 13500 .  -  .  ID=exon00004;Parent=mRNA00002
chr1  .  CDS           10500 10700 .  -  0  ID=cds00003;Parent=mRNA00002
chr1  .  CDS           12000 13500 .  -  0  ID=cds00004;Parent=mRNA00002
```

```bash
$ cat annotations.gff > backup_annotations.gff
```

### 1.4 Concatenar archivos

La concatenación de archivos es útil cuando tienes múltiples archivos de secuencias que deseas combinar en un solo archivo:

```bash
$ cat seq1.fasta seq2.fasta > combined_sequences.fasta
```

### 1.5 Mostrar número de líneas

En el caso de archivos grandes, como archivos FASTQ que contienen lecturas de secuencias, puede ser útil contar el número de líneas para verificar la integridad del archivo:

```bash
$ cat > reads.fastq
@SEQ_ID_1
GATTTGGGGTTCAAAGCAGTATCGATCAAATAGTACGCGCG
+
IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII
@SEQ_ID_2
TTGGCCGATGCCGTAGCAGGTTGCAAAGGTCAGAGTGCGAA
+
IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

$ cat -n reads.fastq
1	@SEQ_ID_1
2	GATTTGGGGTTCAAAGCAGTATCGATCAAATAGTACGCGCG
3	+
4	IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII
5	@SEQ_ID_2
6	TTGGCCGATGCCGTAGCAGGTTGCAAAGGTCAGAGTGCGAA
7	+
8	IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII
```

El comando cat es fundamental en bioinformática para la gestión de archivos de datos y secuencias, facilitando la manipulación y revisión de grandes volúmenes de datos.

## 2. Comando paste

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

```bash
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

Esto es útil cuando se desean separar las columnas fusionadas con diferentes delimitadores para una mejor visualización o procesamiento posterior.

## 3. Comando join

Modo de uso: 

```
$ join <opciones> <fichero1> <fichero2>
```

### 3.1 Practiquemos

```bash
# Crearemos nuestros datos en los archivos prueba1.txt y prueba2.txt

$ cat > prueba1.txt # La primera columna es el DNI y la segunda columna el nombre.
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

$ cat > prueba3.txt # La primera columna es el nombre y la segunda columna el DNI.
Mario Neta,11321345
Aquiles Bailo,20324151
Elsa Blazo,12415132
Elena Nito,32412512
César Noso,13241351

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

# La opción -1 especifica el campo del primer archivo que se utilizará para la comparación, mientras que la opción -2 especifica el campo del segundo archivo. Si los archivos no están ordenados por las columnas de comparación, se recomienda usar la opción -t para definir el delimitador y asegurarse de que la salida sea correcta.

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

## 4. Comando diff

Modo de uso: 

```
$ diff <opciones> <fichero1> <fichero2>
```

* Compara línea por línea dos archivos

Opciones:

`-q`: Reporta solo si los archivos difieren.

`-s`: Reporta si los archivos son idénticos.

`-y`: Salida en dos columnas.

`-u` : Formato unificado, más facil de leer.

```
$ cat archivo1.txt
línea 1: Hola
línea 2: Este es un ejemplo
línea 3: Final del archivo

$ cat archivo2.txt
línea 1: Hola
línea 2: Este es un ejemplo modificado
línea 3: Final del archivo

$ diff archivo1.txt archivo2.txt
2c2
< Este es un ejemplo
---
> Este es un ejemplo modificado
```

`2c2` indica que la segunda línea de archivo1.txt es diferente de la segunda línea de archivo2.txt.
`<` muestra el contenido de archivo1.txt.
`>` muestra el contenido de archivo2.txt.

```
$ diff -u archivo1.txt archivo2.txt
--- archivo1.txt  2024-08-14 10:00:00.000000000
+++ archivo2.txt  2024-08-14 10:00:00.000000000
@@ -1,3 +1,3 @@
 Hola
-Este es un ejemplo
+Este es un ejemplo modificado
 Final del archivo
```

Las líneas precedidas por `-` indican que se eliminaron del primer archivo.
Las líneas precedidas por `+` indican que se agregaron en el segundo archivo.

```
$ cat archivo1.txt
línea 1: Hola
línea 2: Final del archivo

$ cat archivo2.txt
línea 1: Hola
línea 2: Este es un ejemplo modificado
línea 3: Adiós

$ diff archivo1.txt archivo2.txt
2a3
> Adiós
```

`2a3`: Después de la línea 2 en archivo1.txt, la línea 3 ha sido añadida en archivo2.txt.

```bash
$ cat archivo1.txt
línea 1: Hola
línea 2: Este es un ejemplo
línea 3: Adiós

$ cat archivo2.txt
línea 1: Hola
línea 2: Este es un ejemplo 

$ diff archivo1.txt archivo2.txt
3d2
< Adiós
```

`3d2`: La línea 3 en archivo1.txt ha sido eliminada en archivo2.txt.

```bash
# Creamos nuestroa archivos distribuciones1.txt

$ cat distribuciones1.txt
CentOS
Red Hat
Ubuntu Debian
Linux Mint
Fedora
OpenSUSE

$ cat distribuciones2.txt
CentOS
Red Hat
Ubuntu Debian
Linux Mint
OpenSUSE
Fedora
 
$ diff -q distribuciones1.txt distribuciones2.txt

$ diff -s distribuciones1.txt distribuciones2.txt

$ diff -u distribuciones1.txt distribuciones2.txt

$ nano distribuciones1.txt
CentOS
Slax
Red Hat
Ubuntu Debian
Linux Mint
Fedora
OpenSUSE

$ nano distribuciones2.txt

CentOS
Red Hat
Debian
Linux Mint
Fedora
Xubuntu

$ diff distribuciones1.txt distribuciones2.txt
0a1
> 
2d2
< Slax
4c4
< Ubuntu Debian
---
> Debian
7c7
< OpenSUSE
---
> Xubuntu

1. Los números de línea correspondientes al primer fichero.
2. Una letra: `a` (add) para añadir, `c` (change) para cambiar, y `d` (delete) para borrar.
3. Los números de línea correspondientes al segundo fichero.
4. Símbolos especiales en los cambios a realizar:
   - El símbolo < son líneas del primer fichero
   - El símbolo > son líneas del segundo fichero
```

```bash
$ diff -y distribuciones1.txt distribuciones2.txt 
							      >
CentOS								CentOS
Slax							  <
Red Hat								Red Hat
Ubuntu Debian				|	Debian
Linux Mint						Linux Mint
Fedora								Fedora
OpenSUSE						|	Xubuntu


$ diff -y distribuciones1.txt distribuciones2.txt --suppress-common-lines
							      >
Slax							  <
Ubuntu Debian				|	Debian
OpenSUSE						|	Xubuntu


$ diff -c distribuciones1.txt distribuciones2.txt 
*** distribuciones1.txt	2024-08-14 19:50:34.231249295 -0500
--- distribuciones2.txt	2024-08-14 19:54:48.828974690 -0500
***************
*** 1,7 ****
  CentOS
- Slax
  Red Hat
! Ubuntu Debian
  Linux Mint
  Fedora
! OpenSUSE
--- 1,7 ----
+ 
  CentOS
  Red Hat
! Debian
  Linux Mint
  Fedora
! Xubuntu

$ diff -u distribuciones1.txt distribuciones2.txt 
--- distribuciones1.txt	2024-08-14 19:50:34.231249295 -0500
+++ distribuciones2.txt	2024-08-14 19:54:48.828974690 -0500
@@ -1,7 +1,7 @@
+
 CentOS
-Slax
 Red Hat
-Ubuntu Debian
+Debian
 Linux Mint
 Fedora
-OpenSUSE
+Xubuntu
```

## 5. Comando cut

Permite seleccionar o cortar secciones de cada línea de un archivo y escribir el resultado en la salida estándar.

Se utiliza para seleccionar partes de una línea por posición de byte, carácter o delimitador.

Se puede usar para extraer datos de archivos:

• csv (comma separated-values)
• tsv (tab separated-values)

### 5.1 Opciones:

`-b`: Para obtener una sección de una línea especificando una posición de byte se utiliza la opción –b y especificando el rango de bytes que nos interesa.

```bash
$ echo 'prueba' | cut -b 2
r

$ $ echo 'prueba' | cut -b 1-2
pr

$ echo 'prueba' | cut -b 1,3
pu
```

`-c`: También podemos seleccionar por carácter con la opción –c Cuando el flujo de entrada de datos se basa en caracteres, -c puede ser una mejor opción que hacer la selección por bytes, ya que a menudo encontramos caracteres que son representados por más de un byte.

```bash
$ echo '@fastqs' | cut -c 1,6
@q

$ echo '@fastqs' | cut -c 1-3
@fa
```

`-d`: Podemos seleccionar secciones del flujo de datos usando un delimitador con la opción `–d`. Por defecto el delimitador que asume el comando es el TAB. Esta opción se usa normalmente junto con la opción -f para especificar el campo o los campos a seleccionar

```bash
# Creamos nuestro archivo names.csv
$ nano names.csv
John,Smith,34,London
Arthur,Evans,21,Newport
George,Jones,32,Truro
```

El delimitador se puede indicar utilizando la opción -d junto al delimitador de interés entre comillas -d ','. 

```bash
$ cut -d ',' -f 1 names.csv
John
Arthur
George

$ cut -d ',' -f 1,4 names.csv
John,London
Arthur,Newport
George,Truro

$ cut -d ',' -f 1 names.csv # ¿Qué obtenemos?

$ cut -f 1 names.csv # ¿Qué obtenemos?
```

## 6. Secuencias de escape

Una secuencia de escape está formado por el carácter \ seguido de una letra o de una combinación de dígitos. Son utilizadas para acciones como nueva líneas, tabulador, comillas, etc

| Secuencia | Significado                                      |
|-----------|-------------|
| `\n`      | Nueva línea |
| `\t`      | Tabulador   |

## 7. Comando sort

Permite ordenar líneas de archivos de entrada utilizando ciertos criterios de ordenamiento 

1. Verificar si los datos de entrada están ordenados (`-c`)
2. Ordenar alfabéticamente
3. Clasificar en orden inverso (`-r`)
4. Ordenar por número (`-n`)
5. Ordenar y eliminar duplicados (`-u`)
6. Ordenar por elementos que no están al principio de la línea (`-k`)

### 7.1 Comando sort -> history

```bash
$ cat > genes.txt
pax4
sox13
sox13
baf1
tcf4

$ sort -c genes.txt 
sort: genes.txt:4: disorder: baf1

$ sort genes.txt 
baf1
pax4
sox13
sox13
tcf4

$ sort -u genes.txt 
baf1
pax4
sox13
Tcf4

$ sort -u genes.txt -o genes_output.txt

$ cat genes_output.txt 
baf1
pax4
sox13
tcf4

$ sort -u genes.txt -o genes_output.txt

$ cat genes_output.txt 
baf1
pax4
sox13
tcf4

$ sort -r genes_output.txt 
tcf4
sox13
pax4
baf1

$ shuf -i 1-10 -n 10 > numeros.txt

$ sort -n numeros.txt 
1
2
3
4
5
6
7
8
9
10

$ ls -lh | head -n5 > ordenar.txt

$ sort -n -k 2 ordenar.txt 
```

## 8. Comando uniq (unique)

`uniq` se utiliza para informar u omitir cadenas o líneas repetidas.

```
uniq <opciones> <fichero>
```

1. Ver la cantidad de veces que se repite una línea (`-c`)
2. Imprimir en pantalla las líneas repetidas, y obviar las no repetidas (`-d`)
3. Imprimir en pantalla las líneas no repetidas (`-u`)
4. Ignorar mayúsculas y minúsculas (`-i`)

```bash
$ cat uniq.txt 
prueba1
prueba1
prueba1
Prueba1
Prueba2
prueba2
test1
test1
Test1

$ uniq -c uniq.txt 
3 prueba1
1 Prueba1
1 Prueba2
1 prueba2
2 test1
1 Test1

$ uniq -u uniq.txt 
Prueba1
Prueba2
prueba2
Test1

$ uniq -d uniq.txt 
prueba1
test1

$ uniq uniq.txt 
prueba1
Prueba1
Prueba2
prueba2
test1
Test1

$ uniq -i uniq.txt 
prueba1
Prueba2
test1
```

¿Qué ocurre si `NO` está ordenado?

```bash
$ cat uniq2.txt
prueba1
prueba1
prueba1
Prueba1
Prueba2
prueba2
test1
test1
Test1
prueba1

$ uniq uniq2.txt 
prueba1
Prueba1
Prueba2
prueba2
test1
Test1
prueba1
```

| No encuentra entradas duplicados que no se encuentren en líneas adyacentes --> _comandos SORT_

## 9. Comando tr (translate)

Se usa para:

1. Cambiar caracteres:
   
  • Mayúsculas a minúsculas o viceversa.
  • Reemplazarlos por otros.
  
3. Borrar caracteres.

`NO` transforma palabras completas - Trabaja caracter por caracter.


### 9.2 Comando tr (translate) -> history

```bash 
$ echo "Esto es una ejemplo de tr para la clase de hoy" | tr 'a' 'A'
Esto es unA ejemplo de tr pArA lA clAse de hoy

$ echo "Esto es una ejemplo de tr para la clase de hoy" | tr 'aeiou' 'AEIOU’
EstO Es UnA EjEmplO dE tr pArA lA clAsE dE hOy

$ cat /etc/passwd | tr "aeiou" "AEIOU" | head

rOOt:x:0:0:rOOt:/rOOt:/bIn/bAsh
bIn:x:1:1:bIn:/bIn:/sbIn/nOlOgIn
dAEmOn:x:2:2:dAEmOn:/sbIn:/sbIn/nOlOgIn
Adm:x:3:4:Adm:/vAr/Adm:/sbIn/nOlOgIn
lp:x:4:7:lp:/vAr/spOOl/lpd:/sbIn/nOlOgIn
sync:x:5:0:sync:/sbIn:/bIn/sync
shUtdOwn:x:6:0:shUtdOwn:/sbIn:/sbIn/shUtdOwn
hAlt:x:7:0:hAlt:/sbIn:/sbIn/hAlt
mAIl:x:8:12:mAIl:/vAr/spOOl/mAIl:/sbIn/nOlOgIn
OpErAtOr:x:11:0:OpErAtOr:/rOOt:/sbIn/nOlOgIn

$ tr "aeiou" "AEIOU" < /etc/passwd | head

$ echo "Esto es una ejemplo de tr para la clase de hoy" | tr ‘a-z' ‘A-Z’
ESTO ES UNA EJEMPLO DE TR PARA LA CLASE DE HOY

$ echo "Esto es un ejemplo de tr para la clase de hoy" | tr -d "e"
Esto s un jmplo d tr para la clas d hoy

$ echo "Esto es un ejemplo de tr para la clase de hoy" | tr -d "Ee"
sto s un jmplo d tr para la clas d hoy

$ echo "AAA TTTTT GGGG AAA TTTT NNN"
AAA TTTTT GGGG AAA TTTT NNN

$ echo "AAA TTTTT GGGG AAA TTTT NNN" | tr -s " "
AAA TTTTT GGGG AAA TTTT NNN

$ echo "AAA TTTTT GGGG AAA TTTT NNN" | tr -c "ATGN" "-"
AAA-TTTTT-GGGG-AAA-TTTT-NNN-
```

## 10. Comando wc (word count)

```
wc <opciones> <fichero(s)>
```

Sin opciones : cuenta líneas, palabras y caracteres)

Opciones:

`-c`: bytes
`-m`: caracteres
`-l`: líneas
`-w`: palabras
`-L`: longitud de la línea más larga

```bash
$ cat sequences.txt 
1 ACGT
2 ACGT
3 TTTGACA
$ wc sequences.txt 
```

¿Cuál será el resultado? --> Se cuentan los caracteres que se ven y aquellos que no se ven

## 11. Comand rev

rev se utiliza para invertir las líneas de texto en función de los caracteres

```
rev <opciones> <fichero>
```

```bash
$ echo "ATGC" | rev
CGTA

$ cat > sequences.txt
AAATTT
GGGCC
CATG

$ rev sequences.txt 
TTTAAA
CCGGG
GTAC
```

# 12. Comando fold

Permite dividir las líneas en un ancho especificado

```bash
$ cat fold1.txt 
Aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa

$ fold –w 80 fold1.txt 
Aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
Aaaaaaaaaaaaaaaaaa

$ fold –w 30 fold1.txt 
Aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```
