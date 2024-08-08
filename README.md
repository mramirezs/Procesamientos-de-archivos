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

La opción -1 especifica el campo del primer archivo que se utilizará para la comparación, mientras que la opción -2 especifica el campo del segundo archivo. Si los archivos no están ordenados por las columnas de comparación, se recomienda usar la opción -t para definir el delimitador y asegurarse de que la salida sea correcta.

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
Ubuntu Debian
Linux Mint
OpenSUSE
Fedora

$ diff distribuciones1.txt distribuciones2.txt
2d1
< Slax
7c6
< OpenSUSE
---
> Xubuntu
```

1. Los números de línea correspondientes al primer fichero.
2. Una letra: `a` (add) para añadir, `c`c (change) para cambiar, y `d` (delete) para borrar.
3. Los números de línea correspondientes al segundo fichero.
4. Símbolos especiales en los cambios a realizar:
   - El símbolo < son líneas del primer fichero
   - El símbolo > son líneas del segundo fichero

```bash
$ diff -y distribuciones1.txt distribuciones2.txt 
CentOS    CentOS
Slax       <
Red Hat    Red Hat
Debian     Debian
Linux Mint Linux Mint
Fedora     Fedora
OpenSUSE   | Xubuntu

$ diff -y distribuciones1.txt distribuciones2.txt --suppress-common-lines
Slax     <
OpenSUSE | Xubuntu

$ diff -c distribuciones1.txt distribuciones2.txt 
*** distribuciones1.txt 2023-05-04 15:03:23.053374252 +0200
--- distribuciones2.txt 2023-05-04 15:03:41.723395284 +0200
***************
*** 1,7 ****
CentOS
-Slax
Red Hat
Debian
Linux Mint
Fedora
!OpenSUSE
--- 1,6 ----
CentOS
Red Hat
Debian
Linux Mint
Fedora
!Xubuntu
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
