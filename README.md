# GNS3-Hipervisores

---

##  Introducción

Este proyecto tiene como objetivo investigar e implementar laboratorios de redes avanzados utilizando **GNS3** sobre **Windows 11**, integrando hipervisores de tipo 1 (**VMware ESXi**) y tipo 2 (**VirtualBox**).
Se busca comprender la arquitectura de virtualización, optimizar el rendimiento y solucionar problemas comunes en entornos reales.

---

##  1. Arquitectura de Virtualización en Windows 11

###  Aislamiento de Núcleo (Core Isolation) y VBS

El **Core Isolation** y **VBS (Virtualization-Based Security)** son mecanismos de seguridad en Windows 11 que utilizan virtualización para proteger el sistema.
Estas funciones pueden activar **Hyper-V**, lo que interfiere con herramientas como GNS3 o VirtualBox.
**Solución:**
Desactivar el aislamiento de núcleo y la integridad de memoria desde la seguridad de Windows.

---

### ⚙️ Activación de VT-x / AMD-V

**Pasos:**

1. Reiniciar el PC
2. Ingresar a BIOS/UEFI
3. Activar:
   * Intel VT-x
   * AMD-V

**Verificación en Windows:**
```powershell
systeminfo
```
Debe aparecer:
**Virtualization Enabled In Firmware: Yes**

---

##  2. GNS3 VM: El Motor de Simulación

###  KVM (Kernel-based Virtual Machine)

KVM es una tecnología de virtualización basada en Linux que permite ejecutar máquinas virtuales con alto rendimiento.
En GNS3 debe aparecer:
**KVM support available: True**

Si aparece en False:
* Virtualización no activada
* Conflicto con Hyper-V

---

###  Configuración de Recursos

| Recurso | Recomendación |
| ------- | ------------- |
| CPU     | 2 - 4 núcleos |
| RAM     | 4 - 8 GB      |
| Disco   | 20 GB mínimo  |

**Regla:** No usar más del 50% de tu PC.

---

##  3. Integración con VirtualBox (Local)

###  Configuración de Red (Host-Only)

Pasos:

1. Crear adaptador Host-Only
2. Asignar IP (ej: 192.168.56.1)
3. Conectar la GNS3 VM

---

###  Modo Promiscuo (Promiscuous Mode)

Permite capturar todo el tráfico de red.

**Importancia:**
Necesario para tráfico de capa 2 (switches, VLANs)
**Configuración:**
Modo promiscuo → **Permitir todo**

---

##  4. Integración con VMware ESXi (Remoto)

###  Arquitectura Cliente-Servidor

* Cliente: Tu laptop (GNS3 GUI)
* Servidor: GNS3 VM en ESXi
**Ventaja:** Mejor rendimiento

---

###  Seguridad en vSwitch

Configurar:

| Parámetro           | Valor  |
| ------------------- | ------ |
| Promiscuous Mode    | Accept |
| MAC Address Changes | Accept |
| Forged Transmits    | Accept |

---

##  5. Troubleshooting

| Error             | Causa                      | Solución                          |
| ----------------- | -------------------------- | --------------------------------- |
| KVM False         | Virtualización desactivada | Activar VT-x y desactivar Hyper-V |
| Sin conexión      | Red mal configurada        | Activar modo promiscuo            |
| Error puerto 3080 | Firewall bloqueando        | Abrir puertos                     |
| uBridge error     | Permisos                   | Ejecutar como administrador       |

---

##  Comparación de Hipervisores

| Característica | VirtualBox | VMware ESXi |
| -------------- | ---------- | ----------- |
| Tipo           | Tipo 2     | Tipo 1      |
| Rendimiento    | Medio      | Alto        |
| Uso            | Local      | Servidor    |

---

##  Diagramas

Coloca aquí tus imágenes:

![Topología](img/topologia.png)

---

## 📎 Referencias

- https://www.telectronika.com/articulos/ti/que-es-gns3/
- https://www.telectronika.com/tutoriales/gns3-tutorial-instalacion-configuracion/
- https://www.telectronika.com/tutoriales/gns3-vm-setup-wizard/

---

## 📚 Conclusiones

* GNS3 permite simulaciones avanzadas
* La virtualización es clave
* ESXi ofrece mejor rendimiento
* El troubleshooting es esencial

---

## 👨‍💻 Autor

Luis Miguel Huamani Garcia
