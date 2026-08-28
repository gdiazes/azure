
##  1. Tabla Comparativa de Licencias y Activación de Prueba Gratuita (*Trial*)

### 1.1 Matriz de Capacidades por Nivel de Licencia

| Característica / Paso de la Guía | Entra ID Free (Cuenta Básica Gratuita) | Entra ID P1 / P2 *(o Trial de 30 días)* | Azure Subscription (Capa Gratuita) |
| :--- | :---: | :---: | :---: |
| **Paso 2.1: Grupos Dinámicos** (Reglas por atributos) |  *No disponible (Bloqueado)* |  **Totalmente compatible** | N/A (Función de Identidad) |
| **Paso 2.2: Grupos Asignados** (Seguridad estática) |  **Totalmente compatible** |  **Totalmente compatible** | N/A (Función de Identidad) |
| **Paso 3: Usuarios Invitados B2B** |  **Gratis** (hasta 50.000 MAU) |  **Gratis** (con funciones avanzadas) | N/A (Función de Identidad) |
| **Paso 4: Azure RBAC** (Lector, Contribuyente en RG) |  **Totalmente compatible** |  **Totalmente compatible** |  **Incluido sin costo** |
| **Gobernanza Avanzada** (PIM, Revisiones de acceso) |  *No disponible* |  **Incluido en P2** | N/A |

---

### 1.2 Paso a Paso: Cómo Activar la Prueba Gratuita de Microsoft Entra ID P2 (Sin Costo)

Para desbloquear la creación de **Grupos Dinámicos** en tu laboratorio sin pagar licencias:

1. Ingresa al centro de administración en [entra.microsoft.com](https://entra.microsoft.com/) o [portal.azure.com](https://portal.azure.com/).
2. En el menú de navegación izquierdo, dirígete a:  
   **Identidad (Identity)** > **Facturación (Billing)** > **Licencias (Licenses)**.
3. En la sección lateral, haz clic en **Probar o comprar (Try / Buy)** o **Todos los productos (All products)**.
4. Ubica la tarjeta de **Microsoft Entra ID P2** y haz clic en la opción desplegable **Prueba gratuita (Free trial)** > **Activar (Activate)**.
5. *(Opcional)*: Ve a **Licencias** > **Todos los productos** > selecciona **Microsoft Entra ID P2** > haz clic en **+ Asignar (+ Assign)** y asigna una licencia a tu cuenta de administrador y a los usuarios con los que probarás las reglas dinámicas.

---

##  Paso 2: Crear Grupos en Microsoft Entra ID (Dinámicos y Asignados)

```
entra.microsoft.com / portal.azure.com
 └── Identity (Identidad)
      └── Groups (Grupos)
           └── All groups (Todos los grupos) -> + New group (+ Nuevo grupo)
```

---

### 2.1 Diseñar y Crear el Grupo Dinámico (Microsoft 365)
Los grupos dinámicos administran automáticamente la membresía de los usuarios leyendo atributos en tiempo real desde el directorio.

1. Navega a **Identidad** > **Grupos** > **Todos los grupos** y haz clic en **+ Nuevo grupo (+ New group)**.
2. Configura los campos:
   * **Tipo de grupo (Group type):** `Microsoft 365` *(habilita buzón compartido, Teams y SharePoint colaborativo)*.
   * **Nombre del grupo:** `M365-Dynamic-ProjectManagers`.
   * **Descripción:** *Grupo dinámico para todos los Gerentes de Proyecto de la organización.*
   * **Tipo de pertenencia (Membership type):** Selecciona **Usuario dinámico (Dynamic User)**.
3. Definir la regla de pertenencia:
   * Haz clic en **Agregar consulta dinámica (Add dynamic query)**.
   * En el generador de reglas:
     * **Propiedad (Property):** `jobTitle`
     * **Operador (Operator):** `Equals`
     * **Valor (Value):** `Project Manager` *(o el título exacto asignado a los usuarios, ej. `Gerente de Proyecto`)*.
   * **Sintaxis de la regla (Rule Syntax):**
     ```text
     user.jobTitle -eq "Project Manager"
     ```
     *(Filtro avanzado para exigir además que la cuenta esté activa: `(user.jobTitle -eq "Project Manager") -and (user.accountEnabled -eq true)`).*
4. Haz clic en **Guardar (Save)** y luego en **Crear (Create)**.
5. *Nota técnica:* El motor de sincronización de Entra ID evalúa los cambios en segundo plano y toma entre 2 y 15 minutos en reflejar los miembros en el grupo.

---

### 2.2 Establecer Grupo con Pertenencia Asignada (Security Group)
Para equipos con roles técnicos o privilegios elevados (como soporte de TI), la asignación estática previene que un cambio accidental en los atributos de un perfil otorgue accesos críticos.

1. En la vista de **Todos los grupos**, haz clic en **+ Nuevo grupo**.
2. Configura los parámetros:
   * **Tipo de grupo:** `Seguridad (Security)`.
   * **Nombre del grupo:** `SEC-Azure-IT-Support`.
   * **Descripción:** *Grupo asignado estáticamente para el equipo de IT Support.*
   * **Tipo de pertenencia:** `Asignado (Assigned)`.
3. Asignación de miembros:
   * Haz clic en el enlace **Sin miembros seleccionados (No members selected)**.
   * Busca y selecciona las cuentas de los usuarios del equipo de soporte.
4. Haz clic en **Seleccionar** y luego en **Crear**.

---

##  Paso 3: Administrar Usuarios Invitados (Azure B2B Collaboration)

Permite la colaboración segura con usuarios externos (consultores, auditores, proveedores) sin necesidad de crear credenciales locales independientes.

```
entra.microsoft.com / portal.azure.com
 └── Identity (Identidad)
      └── Users (Usuarios)
           └── All users (Todos los usuarios) -> + New user -> Invite external user
```

---

### 3.1 Invitación y Restricciones del Usuario Externo
1. Ve a **Identidad** > **Usuarios** > **Todos los usuarios**.
2. Haz clic en **+ Nuevo usuario (+ New user)** > **Invitar a usuario externo (Invite external user)**.
3. Completa los datos:
   * **Nombre:** `Consultor Externo`
   * **Dirección de correo electrónico:** *Ingresa una cuenta de correo secundaria que controles (ej. cuenta de Gmail u Outlook personal)*.
   * **Mensaje personalizado:** *Bienvenido al entorno de pruebas de Azure. Tu acceso se encuentra restringido a tareas de lectura/auditoría.*
4. **Propiedades de seguridad:**
   * **Tipo de usuario (User type):** `Invitado (Guest)`.
   * Mantén las directivas por defecto de Entra ID para invitados (bloqueo para enumerar el directorio completo).
5. Haz clic en **Invitar**.

### 3.2 Canje y Aceptación de la Invitación
1. Abre la bandeja de entrada del correo externo invitado.
2. Abre el mensaje recibido de *Microsoft Invitations* y presiona **Aceptar invitación (Accept invitation)**.
3. Completa el flujo de autenticación y consentimiento. El estado de la cuenta cambiará a `Accepted` en el directorio.

---

##  Paso 4: Asignar Roles y Permisos (Azure RBAC)

La asignación de permisos sobre la infraestructura se realiza mediante **Azure RBAC** asignando roles a **grupos** en lugar de usuarios individuales, bajo el principio de menor privilegio (*Least Privilege*).

```
Azure Portal (portal.azure.com)
 └── Resource Groups (Grupos de recursos) / Subscriptions
      └── [Seleccionar recurso objetivo]
           └── Access Control (IAM) -> + Add -> Add role assignment
```

---

### 4.1 Asignación para el Grupo de TI (`SEC-Azure-IT-Support`)
1. En el Portal de Azure, entra al **Grupo de recursos** de destino (ej. `rg-produccion-demo`) o al nivel de **Suscripción**.
2. En el menú lateral izquierdo, haz clic en **Control de acceso (IAM)** > **+ Agregar (+ Add)** > **Agregar asignación de roles (Add role assignment)**.
3. **Pestaña Rol (Role):** Busca y selecciona `Virtual Machine Contributor` (o `Contributor` si el alcance abarca todos los servicios del grupo). Haz clic en *Siguiente*.
4. **Pestaña Miembros (Members):**
   * *Asignar acceso a:* **Usuario, grupo o entidad de servicio**.
   * Haz clic en **+ Seleccionar miembros** y selecciona el grupo `SEC-Azure-IT-Support`.
5. Haz clic en **Revisar y asignar (Review + assign)**.

---

### 4.2 Asignación de Permiso Mínimo para el Usuario Invitado (Auditoría/Lectura)
1. En el mismo menú **IAM** del grupo de recursos:
2. Haz clic en **+ Agregar** > **Agregar asignación de roles**.
3. **Pestaña Rol:** Busca y selecciona `Reader` (Lector). Haz clic en *Siguiente*.
4. **Pestaña Miembros:**
   * Haz clic en **+ Seleccionar miembros** y busca al usuario invitado (`Consultor Externo`).
5. Haz clic en **Revisar y asignar**.

---

##  Evaluación y Reflexión

### 1. Pruebas de Validación y Comportamiento Esperado
Abre navegadores en **modo incógnito/privado** e inicia sesión con cada cuenta para validar el aislamiento:

| Identidad de Prueba | Acción / Escenario | Resultado Esperado |
| :--- | :--- | :--- |
| **Usuario con `jobTitle = "Project Manager"`** | Revisar la lista de miembros de `M365-Dynamic-ProjectManagers` | El usuario aparece listado automáticamente tras completarse la evaluación dinámica. |
| **Miembro de `SEC-Azure-IT-Support`** | Iniciar/detener máquinas virtuales dentro del Resource Group | Acción ejecutada exitosamente; denegado si intenta modificar accesos en la pestaña IAM. |
| **Usuario Invitado (`Guest`)** | Navegar al Resource Group e intentar crear o modificar un recurso | Visualiza configuraciones y métricas, pero cualquier intento de despliegue arroja error **403 Forbidden / Unauthorized**. |

---

### 2. Documentación Técnica de la Configuración

* **Matriz de Control de Acceso e Identidad:**
  * `M365-Dynamic-ProjectManagers`: Tipo Dinámico M365 | Regla: `user.jobTitle -eq "Project Manager"`.
  * `SEC-Azure-IT-Support`: Tipo Seguridad Asignado | Rol RBAC: `Virtual Machine Contributor` sobre el ámbito del Resource Group.
  * `Consultor Externo (Guest)`: Tipo B2B Guest | Rol RBAC: `Reader` sobre el ámbito del Resource Group.
* **Políticas de Seguridad Implementadas:**
  * Uso de grupos como contenedores de seguridad para evitar la dispersión de permisos (*Access Sprawl*).
  * Principio de Mínimo Privilegio: Cuentas externas restringidas a permisos de solo lectura sin acceso a la jerarquía superior.

---

### 3. Reflexión de Especialista y Recomendaciones para Producción

#### Desafíos Encontrados y Soluciones:
1. **Localización de herramientas en el portal:**  
   *Desafío:* La búsqueda genérica "Identity" muestra submódulos de seguridad en vez del servicio de administración del tenant.  
   *Solución:* Usar de forma consistente el portal dedicado `entra.microsoft.com` para la capa de identidad y `portal.azure.com` para recursos de infraestructura.
2. **Latencia de pertenencia dinámica:**  
   *Desafío:* Los cambios en los atributos del usuario no se reflejan instantáneamente en el grupo.  
   *Solución:* Utilizar la opción integrada **"Validar reglas (Validate rules)"** dentro del editor dinámico antes de guardar la consulta.
3. **Cuentas invitadas inactivas (*Stale Guests*):**  
   *Desafío:* Invitados que conservan permisos tras finalizar su contrato.  
   *Solución:* Programar revisiones de acceso automáticas (*Access Reviews* de Microsoft Entra ID Governance) cada 30 o 90 días.

#### Recomendaciones para un Entorno Enterprise:
* **Just-In-Time (JIT) vía PIM:** Los administradores de TI no deben tener roles de `Contributor` permanentemente activos; deben activarlos bajo demanda con justificación de ticket y aprobación MFA.
* **Políticas de Acceso Condicional (Conditional Access):** Forzar autenticación multifactor (MFA) a todos los usuarios invitados y bloquear conexiones desde ubicaciones no autorizadas o equipos no administrados (*non-compliant*).
* **Infraestructura como Código (IaC):** Declarar grupos, reglas dinámicas y asignaciones RBAC mediante plantillas de **Terraform** o **Bicep** para mantener trazabilidad y control de versiones en repositorios Git.
