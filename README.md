# Reporte Técnico: Mi Primer Laboratorio Seguro de Ciberseguridad

## Introducción y Objetivo
Este documento detalla la creación, configuración y endurecimiento (hardening) de una Máquina Virtual (VM) utilizada como entorno aislado de prácticas de ciberseguridad.

---

## Parte I: Procedimiento para la Creación del Laboratorio
> **Nota previa:** Se requiere contar con VirtualBox instalado y la imagen ISO del sistema operativo objetivo.

1. **Apertura de VirtualBox:** Iniciar la aplicación y seleccionar la opción **Nueva**.
2. **Configuración Inicial:** Asignar un nombre descriptivo a la máquina virtual, indicar la ruta de la ISO y seleccionar el tipo y versión del Sistema Operativo.
3. **Asignación de Recursos:** Configurar los recursos mínimos recomendados para garantizar estabilidad (mínimo 2 GB de RAM y 2 CPU).
   ![IMG1](https://github.com/montillae/ciberseguridad-lab-hardening-vbox/blob/f883211693e709d0fd106167da91079a5f3aa562/img/01_Configuracion_VM.png
)
5. **Principio de Menor Privilegio (Gestión de Cuentas):**
   * Crear o configurar una cuenta de **Usuario Estándar / Invitado** (*Guest*) con permisos limitados para el trabajo cotidiano, evitando operar de forma continua como Administrador (`root`).
   * *Ruta en Windows:* **Administración de equipos > Usuarios y grupos locales > Usuarios**.
  ![Img2](https://github.com/montillae/ciberseguridad-lab-hardening-vbox/blob/f883211693e709d0fd106167da91079a5f3aa562/img/02_Usuario_Estandar.png)
---

## Parte II: Configuración de Red Aislada y Comparativa de Modos

Para mitigar riesgos durante pruebas con malware o análisis de tráfico, la VM debe estar aislada de la red local física del Host.

* **Configuración aplicada:** Se seleccionó **Red Interna** / **NAT** en **Configuración de la VM > Red**.
  ![Img3](https://github.com/montillae/ciberseguridad-lab-hardening-vbox/blob/f883211693e709d0fd106167da91079a5f3aa562/img/03_Red_Aislada.png)
* **Riesgo del modo Puente (*Bridged*):** No se recomienda el modo Puente para prácticas con amenazas reales, ya que coloca a la VM como un nodo visible dentro de la red física doméstica o corporativa, permitiendo la propagación no deseada de malware o escaneos.

**Tabla comparativa:** Cuando usar cada escenario.

| Modo de Red | Comunicación VM → Internet | Comunicación Host ↔ VM | Comunicación entre VMs | Visibilidad en la Red Física | Escenario de Uso Real |
| --- | --- | --- | --- | --- | --- |
| **NAT (Por defecto)** | Sí | No (requiere *Port Forwarding*) | No | No | Navegación e instalación de actualizaciones/paquetes de forma aislada. |
| **Red Interna (Internal Network)** | No | No | Sí (solo en la misma red interna) | No | Laboratorios de Malware / Pentesting. Aislamiento total del Host y la red física. |
| **Adaptador Solo-Anfitrión (Host-Only)** | No | Sí | Sí | No | Análisis de servicios locales, SSH o servidores web de prueba gestionados desde el Host. |
| **Puente (Bridged Adapter)** | Sí | Sí | Sí | Sí | Simulación de servidores de producción que requieren IP en la red física real. |

---

## Parte III: Importancia de las Guest Additions

Las **Guest Additions** de VirtualBox son un conjunto de controladores y aplicaciones del sistema que optimizan el rendimiento del sistema operativo huésped y mejoran la usabilidad de la VM.

### Funcionalidades clave:
* **Integración del puntero del ratón y pantalla dinámica:** Permite ajustar la resolución automáticamente al redimensionar la ventana.
* **Carpetas compartidas y portapapeles bidireccional:** Facilita el intercambio seguro de archivos e información entre el Host y la VM.
* **Sincronización horaria:** Mantiene la hora de la VM sincronizada con la máquina host, aspecto clave para la correlación de registros y logs de seguridad.

---

## Parte IV: Blindaje mediante Actualizaciones (Windows Update)
1.	Ve a Configuración > Windows Update.
2.	Haz clic en Buscar actualizaciones.
3.	Se adjunta captura donde se observa que el sistema está buscando actualizaciones.

  ![IMG4](https://github.com/montillae/ciberseguridad-lab-hardening-vbox/blob/f883211693e709d0fd106167da91079a5f3aa562/img/04_Windows_Update.png)

---

## Parte V: Creación del Snapshot Inicial ("Máquina del Tiempo")

El objetivo de un *Snapshot* (Instantánea) es congelar el estado del sistema tras aplicar el hardening inicial. Esto permite restaurar la VM a un punto seguro si el sistema operativo se corrompe durante una práctica.

1. Apagar la máquina virtual completamente.
2. Ir al panel de **Instantáneas / Snapshots**, hacer clic en **Tomar**.
3. Nombre recomendado: `Clean Install - Hardening applied`.
4. Añadir una descripción detallada con la fecha y las configuraciones aplicadas.
   ![IMG5](https://github.com/montillae/ciberseguridad-lab-hardening-vbox/blob/f883211693e709d0fd106167da91079a5f3aa562/img/05_Snapshot_Inicial.png)
