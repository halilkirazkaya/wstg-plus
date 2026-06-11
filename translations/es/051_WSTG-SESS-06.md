## WSTG-SESS-06 — Pruebas de la Funcionalidad de Cierre de Sesión (Logout)

La funcionalidad de cierre de sesión es un control de seguridad crítico diseñado para terminar la sesión de un usuario e invalidar los identificadores de sesión asociados tanto en el lado del cliente como en el del servidor. El fallo al implementar correctamente el cierre de sesión permite ataques de Session Fixation o Hijacking, ya que un atacante que obtenga acceso a una máquina después de que un usuario haya "cerrado sesión" podría reutilizar el token de sesión que aún permanece activo. Los pentesters evalúan esto verificando si la cookie de sesión se elimina del navegador, si el estado de la sesión en el servidor se destruye explícitamente y si la navegación con el botón "Atrás" permite el acceso a datos sensibles almacenados en caché. Una implementación de logout segura garantiza que, una vez activada la acción, ninguna solicitud posterior con el antiguo token de sesión sea autorizada por la aplicación.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Herramientas:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### ¿Existe un mecanismo de cierre de sesión funcional y accesible?
- [ ] Sí — el botón de logout existe y activa una solicitud de terminación  
- [ ] No — el botón de logout **no aparece** o **no** activa un evento de terminación  

### ¿Se invalida el identificador de sesión en el lado del servidor?
- [ ] Sí — el servidor rechaza todas las solicitudes posteriores que utilicen el antiguo token de sesión  
- [ ] No — el servidor **continúa aceptando** el antiguo token de sesión después del cierre de sesión  

### ¿La aplicación elimina las cookies de sesión en el navegador al cerrar la sesión?
- [ ] Sí — las cookies se sobrescriben con una fecha de expiración pasada o un valor vacío  
- [ ] Sí — las cookies permanecen pero **ya no son válidas** en el servidor  
- [ ] No — las cookies permanecen en el navegador y **conservan** sus valores originales  

### ¿Se puede acceder a contenido autenticado sensible mediante el botón 'Atrás' del navegador después del cierre de sesión?
- [ ] No — los encabezados `Cache-Control: no-store` o similares impiden ver páginas sensibles tras el cierre de sesión  
- [ ] Sí — las páginas sensibles están en caché y **pueden** ser visualizadas mediante la navegación después de que la sesión ha terminado  

---