## WSTG-CONF-11 — Probar el Almacenamiento en la Nube

Las pruebas de desconfiguraciones en el almacenamiento en la nube implican identificar y auditar servicios de almacenamiento de objetos accesibles públicamente, tales como Amazon S3, Azure Blobs o Google Cloud Storage. Estos recursos a menudo contienen datos sensibles, incluyendo respaldos, código fuente, PII (información de identificación personal) o archivos de configuración que quedan expuestos debido a Listas de Control de Acceso (ACL) o políticas de Gestión de Identidad y Acceso (IAM) excesivamente permisivas. Los atacantes enumeran nombres de buckets a través del reconocimiento de archivos JavaScript, código fuente HTML y técnicas de Brute Force para encontrar puntos de entrada para la exfiltración de datos o la modificación no autorizada de archivos. La explotación puede tener un impacto significativo, incluyendo brechas de datos completas o la capacidad de servir archivos maliciosos desde un dominio de confianza.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica* |

> *La severidad se vuelve Crítica si la PII, las credenciales o los respaldos de producción son accesibles, o si se permite el acceso de escritura sin autenticación.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### ¿Se han identificado y enumerado los endpoints de almacenamiento en la nube (buckets/containers)?
- [ ] No — no se utilizan o no son descubribles endpoints de almacenamiento en la nube  
- [ ] Sí — los endpoints de almacenamiento se identifican mediante análisis de código o monitoreo de tráfico  
- [ ] Sí — los endpoints de almacenamiento se descubren mediante Brute Force de convenciones de nombres  

### ¿Pueden los usuarios no autenticados listar el contenido del almacenamiento en la nube?
- [ ] No — el listado del bucket está **deshabilitado** y devuelve 403 Forbidden o 404 Not Found  
- [ ] Sí — el listado está **habilitado** pero no hay archivos sensibles presentes  
- [ ] Sí — el listado está **habilitado** y los nombres de archivos/metadatos sensibles son visibles  

### ¿Es posible la descarga de archivos sensibles desde los buckets identificados?
- [ ] No — los archivos están protegidos por políticas IAM o se requiere una autenticación válida  
- [ ] Sí — los archivos son accesibles pero requieren una URL firmada (Signed URL) que **no** puede adivinarse fácilmente  
- [ ] Sí — los archivos sensibles (backups, `.env`, PII) son **públicamente accesibles** para su descarga *(Crítica)*  

### ¿Se permite la carga o modificación de archivos sin autenticación?
- [ ] No — las cargas o modificaciones no autenticadas **no son posibles**  
- [ ] Sí — se requiere carga autenticada pero es posible realizar un **bypass** mediante condiciones de política débiles  
- [ ] Sí — la escritura, sobrescritura o eliminación de archivos sin autenticación es **posible** *(Crítica)*  

### ¿Son restrictivas las políticas de Cross-Origin Resource Sharing (CORS) en el endpoint de almacenamiento?
- [ ] Sí — CORS está **deshabilitado** o restringido a orígenes específicos de confianza  
- [ ] No — CORS está **habilitado** con un comodín `*` (wildcard), lo que permite el acceso no autorizado basado en web  

---