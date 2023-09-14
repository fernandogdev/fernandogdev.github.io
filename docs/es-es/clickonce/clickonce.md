# **SealSign Signature Client (ClickOnce)**

## 1. Introducción

El cliente ClickOncede SealSign viene a sustituir en entornos Windows al Applet de Java. Su despliegue está basado en la tecnología ClickOncede Microsoft que permite el despliegue de aplicaciones por internet. Para más información deberá visitarse el sitio de Microsoft.El cliente es capaz de comunicarse de forma bidireccional con el navegador que ha lanzado la petición de firma para conseguir un comportamiento similar a la integración del Applet con el navegador utilizando JavaScript. Esta comunicación se consigue utilizando SignalRde Microsoft. Para más información sobre SignalRpuede visitarse el sitio oficial de SignalR.

## 2. Requisitos mínimos
- El cliente funciona con .Net Framework 4.5
- Los sistemas operativos soportados son:
  - Windows 7
  - Windows 8
  - Windows 10
  - Windows Server 2008 R2
  - Windows Server 2012
- Los navegadores compatibles con SignalR son:
  - Microsoft Edge
  - Google Chrome a partir de la versión 50
  - Mozilla Firefox a partir de la versión 46

## 3. Tareas comunes
### 3.1. Instalación off-premises
El cliente de ClickOnce está hospedado en los servidores de Factum ID. En la página de descarga se puede instalar tanto el cliente como los prerrequisitos del mismo. Una vez instalado aparecerá el icono en el escritorio.

### 3.2. Instalación on-premises
Si  es  necesario  desplegar  el  cliente  en  otro  servidor  que  no sea  el  de  Factum  Identity,  hay  que  seguir  los siguientes pasos.
- Descomprimir el cliente que se encuentra en la descarga del SDK de la versión 4.6
- Para  modificar  los  archivos  de  despliegue  de ClickOncehay  que  descargar  e  instalar  el SDK  de Windows
- Cambiar  la  URL  de  publicación  con  el  siguiente  comando: mage -u  SealSignClient.application -pu http://[URL]/SealSignClient.application
- Reasignar  el  manifiesto  de  la  aplicación  con: mage -u  SealSignClient.application -AppManifest "Application Files\SealSignClient_1_0_0_0\SealSignClient.exe.manifest"
- Finalmente,  firmar  el  archivo  con  el  comando: mage -sign  SealSignClient.application -cf  [ruta  del certificado] -pwd [contraseña del certificado]

Una vez hechos estos pasos se puede desplegar en el servidor el cliente de ClickOnce. Para que todo funcione correctamente hay que comprobar que el servidor web tiene los siguientes tipos MIME configurados:
- .application –> application/x-ms-application
- .manifest –> application/x-ms-manifest
- .deploy –> application/octet-stream

### 3.3. Configuración del cliente JavaScript
En este tutorialse explica en detalle cómo configurar un entorno con SignalR, y en el ejemplo alojado en el gitHub de Factumse encuentra todo el código necesario para hacerlo funcionar.Hay  que  tener  en  cuenta  que  una  vez  que  el  cliente  se  ha  lanzado  se  queda  escuchando  en  el  puerto  8081 cuando es http y 8082 cuando se ha configurado la conexión por https. En la parte de JavaScript habrá que:

- Referenciar al código JavaScript del hub, situada en la URL: http://localhost:8081/signalr/hubs

- Indicar cuál es la URL del hub. $.connection.hub.url = "http://localhost:8081/signalr";

- El nombre del hub de SignalRes sealSignHub. var hub = $.connection.sealSignHub;

- La  aplicación  realiza  llamadas  a  diferentes métodos del cliente JavaScript, tanto  para notificar que está  realizando  alguna  tarea,  como  para  notificar  que  ha  terminado  esa  tarea,  o  para  indicar  al navegador que debe redireccionar a una URL. Los métodos son:
  - Navigate:la aplicación comunica al cliente que debe navegar a la URL que le pasa por parámetro. Normalmente será una de las URLs que se han definido como de éxito, cancelación, rechazo o error. Uso: hub.client.Navigate = function (url) {  }
  - AsyncOperationStarted:la  aplicación  notifica  al  cliente  que  ha  comenzado  una  operación asíncrona y de larga duración. A partir de este punto el cliente JavaScriptdebería ceder el control al   componente ClickOnce.   Adjunta   un   mensaje   con   los   detalles   de   la   operación.   Uso: hub.client.AsyncOperationStarted = function(message){ }
  - AsyncOperationCompleted:la  aplicación  notifica  al  cliente  que  ya  ha  terminado  y  que  puede tomar el control. Uso: hub.client.AsyncOperationCompleted = function(){ }
  - AsyncOperationInProgress: la aplicación notifica al cliente que ya hay una firma en curso. Uso: hub.client.AsyncOperationInProgress= function(){ }

### 3.4. Configuración de la versión del servidor

El  cliente  soporta  tanto  la  versión  3.2  como  la  4.0  de  SealSign,  pero  hay  que  indicar  qué  versión  se  está utilizando. Para configurar la versión que se está utilizando hay que llamar al método setServerVersioncon alguno de estos dos valores:
  - V32: para utilizar la versión 3.2
  - V40: para utilizar la versión 4.0 o posteriores
  

Si no se llamara a la función, por defecto se utilizará la versión 3.2.

## 4. Casos de uso
### 4.1. Ejecución del cliente y lanzar al arrancar Windows

Una vez instalado el cliente aparecerá el icono en el escritorio, para lanzarlo sólo hay que hacer click en él, aparecerá el siguiente mensaje.

El cliente se puede configurar para que se arranque cuando se inicie sesión en Windows, para ello hay que hacer click con el botón derecho sobre el icono y pulsar sobre la opción “Ejecutar al arrancar el equipo”.

### 4.2. Utilizar conexión SSL
#### 4.2.1. Configuración del certificado
Para poder utilizar una conexión SSL entre la web y el cliente de SealSignhay que instalar un certificado en el equipo cliente y enlazarlo al puerto 8082.

Instalación del certificado en el almacén. El certificado a instalar tiene que contener la clave pública y la clave privada. Para instalarlo hacemos doble click sobre el archivo. Se muestra un asistente para hacer la instalación.

Seleccionamos el almacén “Equipo local” y pulsamos “Siguiente”. En la siguiente pantalla, pulsamos “Siguiente”.

En la siguiente pantalla introducimos la contraseña del certificado y pulsamos “Siguiente”

En la siguiente pantalla marcar la opción “Colocar todos los certificados en el siguiente almacén” y seleccionar el almacén “Personal” y pulsar “Siguiente”.

En la ventana de resumen, pulsar “Finalizar”

Si no hay ningún problema deberíamos ver el siguiente mensaje:

Una vez instalado el certificado, se lanza el administrador de certificados, para ello pulsamos la tecla Windows+  R  e  introducimos  “certlm.msc”,  dentro  del  almacén Personalbuscamos  el  certificado  que  importamos anteriormente.

Haciendo doble click sobre el certificado se mostrarán los detalles del mismo.

En la pestaña detalles seleccionar la propiedad “Huella digital”

Con ese valor, hay que abrir la consola en modo administrador y ejecutar el siguiente comando: 
```netsh http add sslcert certhash=<certificate hash> ipport=0.0.0.0:8082 appid={00112233-4455-6677-8899-AABBCCDDEEFF}
```

Con esta última instrucción se asocia el certificado al puerto 8082.

#### 4.2.2. Utilizar SSL

Para que el cliente utilice una conexión SSL hay que seleccionar la opción .
- Referenciar al código JavaScript del hub, situada en la URL: https://localhost:8082/signalr/hubs
- Indicar cuál es la URL del hub. $.connection.hub.url = "https://localhost:8082/signalr";

### 4.3. Firma Digital
Aquí se describe qué funciones se publican para realizar la firma digital de documentos, así como las funciones del cliente JavaScript que se invocan para notificar el progreso y la finalización del proceso.

#### 4.3.1. Filtrado de certificados

A la hora de realizar la firma digital se pueden filtrar los certificados que se mostrarán en el listado. El filtrado se puede hacer por issuer, por hashy por serial number:
- setCertificateIssuerFilter: Filtra por issuer, recibe como parámetro los issuersválidos separados por ‘|’. Se usa para mostrar únicamente los certificados del DNIe: hub.server.setCertificateIssuerFilter('AC DNIE 001');
- setCertifciateHashFilter: Filtra por hash, recibe como parámetro el hashdel certificado con el que se firmará. Uso:hub.server.setCertificateHashFilter('[HASH]');
- setCertificateSerialFilter: Filtra por el serial numberdel certificado. Uso: hub.server.setCertificateSerialFilter('[SERIAL NUMBER]');

#### 4.3.2. Reinicio de filtros
Para eliminar todos los filtros establecidos a loscertificados hay que llamar a la función resetCertificateFilters.

#### 4.3.3. Reinicio de filtros
Se puede realizar la firma de un documento utilizando un certificado almacenado en local, para ello hay que llamar  a  la  función loadLocalCertificate,  para  dejar  de  usar  ese  certificado  hay  que  llamar  a  la  función clearLocalCertificate.

#### 4.3.4. Reinicio de filtros

#### 4.3.5. Uso de Remote Document Provider

Para utilizar los Remote Document Providerhay que configurar los parámetros de configuración, para ello hay que seguir los siguientes pasos:
- Configurar la versión del servidor a la 4.0 realizando una llamada al método setServerVersioncon el valor ‘V40’.
- Llamar a la función setDSSRemoteProviderConfigurationcon los siguientes parámetros:
  - url: la url donde está alojado el Remote Document Provider.
  - domain: dominio del usuario con el que se va a autenticar la llamada al Remote Document Provider.
  - user: usuariocon el que se va a autenticar la llamada al Remote Document Provider.
  - password: contraseña del usuario con el que se autenticará la llamada al Remote Document Provider.
  

Importante:si llamada necesita ser autenticada, la autenticación será básica.