# Raven
Raven es una herramienta de recopilación de información de Linkedin que pueden utilizar los pentesters para recopilar información sobre los empleados de una organización que utilizan Linkedin.

## Disclaimer

```
Por favor, no use este programa para hacer cosas estúpidas.

El autor no se hace responsable de los daños causados por este programa.

ÚSELO BAJO SU PROPIO RIESGO.
```
## Instalación

Puede usar el binario precompilado, pero también puede compilar desde la fuente.

Debe instalar chromedriver incluso si utiliza un binario precompilado o compilando desde la fuente.

Edite las credenciales en ```config.conf```
```
[creds]
username=USERNAME
password=PASSWORD
[extra]
searchengine=google
```
## Compilando en Linux
```bash
export GOPATH=/YOUR/GOPATH/HERE
cd $GOPATH/src/
git clone https://github.com/0x09AL/raven
go get github.com/chzyer/readline
go get github.com/gorilla/mux
go get github.com/mattn/go-sqlite3
go get github.com/olekukonko/tablewriter
go get gopkg.in/gcfg.v1
go get github.com/sclevine/agouti
go build raven

```
## Instalación de chromedriver

```bash
wget https://chromedriver.storage.googleapis.com/2.41/chromedriver_linux64.zip
unzip chromedriver_linux64.zip
sudo mv -f chromedriver /usr/bin/chromedriver
sudo chown root:root /usr/bin/chromedriver
sudo chmod 0755 /usr/bin/chromedriver
```
## Lanzamientos

[https://github.com/0x09AL/raven/releases](https://github.com/0x09AL/raven/releases)

# Dependencias
* [https://github.com/chzyer/readline](https://github.com/chzyer/readline)	
* [https://github.com/gorilla/mux](https://github.com/gorilla/mux)
* [https://github.com/mattn/go-sqlite3](https://github.com/mattn/go-sqlite3)
* [https://github.com/olekukonko/tablewriter](https://github.com/olekukonko/tablewriter)
* [http://gopkg.in/gcfg.v1](http://gopkg.in/gcfg.v1)
* [https://github.com/sclevine/agouti](https://github.com/sclevine/agouti)

## Cómo funciona

La idea principal es que, dado el nombre de una empresa, busca todas las coincidencias posibles de los empleados de Linkedin en Google y luego extrae sus datos. Basado en eso, puede construir direcciones de correo electrónico en diferentes formatos, exportarlas y también verificarlas en haveibeenpwned.com.

La versión anterior de raven le permitía extraer datos solo después de terminar un escaneo y solo en un formato específico. En caso de que quisiera extraer la misma información pero con un formato de correo electrónico diferente, tenía que volver a ejecutar el escaneo, lo que no era muy práctico.

En esta versión, es posible que, dado un escaneo, pueda exportar los datos tantas veces como lo desee, en diferentes formatos y también verificarlos en haveibeenpwned.com con un solo comando.

# Escanear

Un ``scan``  es el proceso de extraer la información pública de Google y Linkedin y almacenarla en la base de datos.

Para crear un escaneo puede ejecutar el comando ``new scan`` que lo llevará a la instancia de escaneo. Hay algunas propiedades que deben configurarse antes de ejecutar un análisis, como se puede ver a continuación.

<center>
<img src="/images/raven-new-scan.png">
</center>

* ```Scan_id``` - No se puede cambiar, es la identificación de escaneo que se utiliza como PK en la base de datos.
* ```Scan_name``` - el nombre del escaneo, que se utiliza más tarde cuando desea exportar datos.
* ```Company``` - el nombre de la compañía que desea extraer empleados.
* ```Domain``` -  Este es el subdominio del sitio web principal de LinkedIn. Si desea orientar a un país específico, puede especificar el subdominio. Por ejemplo, Albania tiene el subdominio ```al```. n caso de que no conozca el subdominio, use ```www```.
* ```Pages_number``` - el número de páginas de Google para extraer información.

Al ejecutar el comando ```options``` puede ver las propiedades y los valores asignados.

A continuación se muestra un ejemplo de escaneo:

<center>
<img src="/images/raven-new-scan.png">
</center>

Después de configurar las propiedades, puede usar ```start``` para iniciar el escaneo.
El escaneo insertará los datos en la base de datos para que pueda usarlos más tarde.

# Exportar

Después de finalizar el escaneo, puede usar los datos ejecutando ```use (scan_name)```.
Para utilizar este comando, debe estar en la instancia ```main```. Por ejemplo, si terminó un escaneo, debe escribir ```back``` para volver a la instancia principal.
Esto lo llevará a exportar instancia. La instancia de exportación le permite exportar los datos en diferentes formatos y verificarlos en haveibeenpwned.com.

La instancia de exportación tiene 3 propiedades.

* ```Format``` - El formato de los correos electrónicos.
* ```Domain``` - El dominio anexar a los" nombres de usuario ".
* ```Output``` - Nombre de archivo para escribir el resultado.

A continuación se muestran los formatos disponibles. También puede usar ```ALL``` en caso de que desee generar todos los formatos disponibles, y luego usar una herramienta personalizada para verificar las direcciones de correo electrónico.

<center>
<img src="/images/raven-formats.png">
</center>

Después de especificar un formato y un dominio, puede exportarlos usando el comando ```export``` o verificar si se han violado usando el comando ```checkpwned``` como se puede ver a continuación.
<center>
<img src="/images/raven-checkpwned.png">
</center>
