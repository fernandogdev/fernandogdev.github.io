# **Biosigner**
## Introducción
La aplicación móvil Biosigner se puede descargar de los markets tanto de IOS como de Android: 

- Apple Store 
- Play Store 

Esta aplicación permite abrir un documento en PDF y firmarlo. La firma, si el Smartphone tiene un pen permite firmarlo y enviarlo por correo electrónico. 

## Configuración de la App Biosigner
Una vez instalada la aplicación debemos configurarla. Abrimos la aplicación para configurar los parámetros que apuntarán a la instancia en cloud creada para el cliente. 


En ambos casos hay que configurar: 

La versión del servidor 

- 32 para la versión 3.2 de SealSign 
- 40 para la versión 4.0 o superior de SealSign 

Si se quiere llamar a SealSignDSSService se tienen que configurar los parámetros de conexión. 

**URL de SealSign**

    https://.../SealSignBSSService/BiometricSignatureServiceBasic.svc/BSSLB 

**Endpoint de SealSign** 

    BSSLB 

**Usuario** 

    Domain\user 

**Contraseña**  

    Password 

Si se quiere llamar a SealSignDSSFrontEnd (Deprecado) se tiene que configurar los parámetros de conexión FrontEnd. 

**URL del FrontEnd**  

    https://.../SealSignBSSFrontEnd/BiometricSignatureServiceBasic.svc/BSSLB 

**Endpoint del FrontEnd** 

    BSSLB 

**Usuario** 

    Domain\user 

**Contraseña**  

    Password 