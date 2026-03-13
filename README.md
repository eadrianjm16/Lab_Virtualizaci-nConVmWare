# Lab_VirtualizacionConVMware

> Laboratorio completo de virtualización con VMware (Workstation Pro, ESXi 8.x, vCenter 8.x), redes virtuales (vSS/vDS), almacenamiento (VMFS6), automatización por **SSH/ESXi Shell**, snapshots, y comparativa con **Microsoft Hyper‑V**. Incluye guías paso a paso, scripts, comandos clave y 📸 capturas.  

**Autor**: EXXEL ADRIÁN JIBAJA MACEDO  
**Proyecto personal**: Documenta y demuestra mi experiencia diseñando y levantando un entorno de virtualización empresarial “end‑to‑end”.  

---

## 🧭 Índice

- [Visión general](#-visión-general)
- [Arquitectura del Lab](#-arquitectura-del-lab)
- [Capturas destacadas](#-capturas-destacadas)
- [Requisitos de hardware y software](#-requisitos-de-hardware-y-software)
- [Guía de instalación (Workstation → ESXi → vCenter)](#-guía-de-instalación-workstation--esxi--vcenter)
- [Red y almacenamiento](#-red-y-almacenamiento)
- [Gestión por SSH/ESXi Shell (comandos listos)](#-gestión-por-sshesxi-shell-comandos-listos)
- [Automatización con `vmrun` (Workstation Pro)](#-automatización-con-vmrun-workstation-pro)
- [Módulo Hyper‑V: redes y PowerShell útiles](#-módulo-hyper-v-redes-y-powershell-útiles)
- [Estructura del repositorio](#-estructura-del-repositorio)
- [Cómo reproducir el laboratorio (paso a paso)](#-cómo-reproducir-el-laboratorio-paso-a-paso)
- [Roadmap](#-roadmap)
- [Licencia](#-licencia)
- [Créditos y formación](#-créditos-y-formación)

---

## 🚀 Visión general

Este proyecto consolida un **lab de virtualización** que levanta infraestructura tipo datacenter sobre **VMware Workstation Pro** (host) con **VMware ESXi 8.x** y **vCenter 8.x** (appliance), para practicar **networking virtual (vSwitch/vDS, VLANs), almacenamiento VMFS, snapshots, clonación, automatización por SSH** y **operaciones avanzadas**. También incorpora un módulo **Hyper‑V** para comparar conceptos y operar con **PowerShell**.  
La guía incluye **requisitos mínimos**, **hardware real usado**, y **pasos reproducibles**. 


- **Workstation Pro 25H2** como hipervisor de tipo 2 moderno (con mejoras de compatibilidad y nuevas features en la rama 25H2). **Nota**: Workstation Pro 25H2 es **gratuito** para uso comercial/educativo/personal según las notas de la versión. [2](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/_layouts/15/Doc.aspx?sourcedoc=%7BB1F85076-CB91-4ECF-9328-DDBA0BADF030%7D&file=POR%20CONSOLA%20SSH%20-%20Y%20VMTOOLS.docx&action=default&mobileredirect=true)  
- **ESXi 8.0U3e (8.0.3)** y **vCenter 8.0U3** como plataforma de virtualización de tipo 1 y orquestación centralizada. [1](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/Diploma%20EXPERTO%20EN%20VIRTUALIZACI%C3%93N.pdf)

---

## 🖼️ Capturas destacadas

> Crea esta ruta en el repo y sube tus imágenes: `assets/images/`.

- Host Client ESXi #1 (`Esxi1_HostClientWeb.png`)
- Configuración VM ESXi #1 (`Esxi1_ConfigVM.PNG`)
- Redes ESXi #1 (`Esxi1_HostClientRedes.png`)
- VMkernel ESXi #1 / #2 / #3 (`Esxi1_VMKernel.png`, `Esxi2_VMKernel.png`, `Esxi3_VMKernel.png`)
- Config VMkernel (ESXi1/2/3) (`Esxi1_VMKernel_ConfigNetwork.png`, `Esxi2_VMKernel_ConfigNetwork.png`, `Esxi3_VMKernel_ConfigNetwork.png`)
- vSwitch0 / vDS (ESXi3) (`Esxi3_vSwitch0.PNG`, `Esxi3_vDSwitch_1uplink.PNG`)
- vSphere Client Web / VCSA (`VMvCenter_vSphereClientWeb.PNG`, `VMvCenter_VMKernel.png`, `VMvCenter_DSwitch_distribuido.PNG`)
- Servidor DNS / Zonas (`WinServerDNS_ConfigVM.png`, `WinServerDNS_ConfigIP.png`, `WinServerDNS_Zonas.png`)
- Utilidades VMware / Adaptador red (`VMware_Software.PNG`, `VMware_ConfigAdaptadorRed.PNG`)

> Puedes agruparlas por secciones en el README o crear un wiki con walkthrough visual.

---

## 🧰 Requisitos de hardware y software

### Requisitos mínimos (ESXi 8.0U3e)
- **CPU x64** con 2+ núcleos, **VT‑x/AMD‑V** y **NX/XD** habilitado.  
- **RAM**: 8 GB (mínimo); recomendado 12–32 GB según el número de VMs.  
- **Red**: al menos 1 NIC 1 GbE.  
- **Arranque**: UEFI preferible.  
- **Storage**: dispositivo de arranque persistente ≥32 GB y datastore adicional (recomendado SSD ≥128 GB). [1](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/Diploma%20EXPERTO%20EN%20VIRTUALIZACI%C3%93N.pdf)

### Equipo real usado (host del lab)
- **AMD Ryzen 7 5825U (16 CPUs lógicos)** con AMD‑V.  
- **32 GB RAM DDR4**, **NVMe 500 GB**, **UEFI**, **Windows 10 Pro 21H2**, **VMware Workstation Pro 25H2**. [1](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/Diploma%20EXPERTO%20EN%20VIRTUALIZACI%C3%93N.pdf)

> La documentación incluye además **requisitos por tamaño** para **vCenter 8.x** (Tiny → XL), consideraciones de **DNS/NTP** y **red estática/FQDN**. [1](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/Diploma%20EXPERTO%20EN%20VIRTUALIZACI%C3%93N.pdf)

---

## 🧱 Guía de instalación (Workstation → ESXi → vCenter)

1) **VMware Workstation Pro 25H2**  
   - Instalación estándar en Windows o Linux.  
   - Novedades clave 25H2 (soporte v22 HW, USB 3.2, herramientas como `dictTool`) y **licenciamiento gratuito** para uso comercial/educativo/personal. [2](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/_layouts/15/Doc.aspx?sourcedoc=%7BB1F85076-CB91-4ECF-9328-DDBA0BADF030%7D&file=POR%20CONSOLA%20SSH%20-%20Y%20VMTOOLS.docx&action=default&mobileredirect=true)

2) **Despliegue de ESXi 8.0.3 (nested) en Workstation**  
   - Crear VM (compatibilidad moderna), montar ISO ESXi 8.0U3e, instalar, poner **IP estática**, y **crear datastore** adicional en un **segundo disco** (p. ej., 200 GB thin). [1](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/Diploma%20EXPERTO%20EN%20VIRTUALIZACI%C3%93N.pdf)

3) **Acceso Host Client & validación**  
   - Entrar por navegador al **Host Client** con `root` y la contraseña creada y validar redes/almacenamiento. [1](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/Diploma%20EXPERTO%20EN%20VIRTUALIZACI%C3%93N.pdf)

4) **Despliegue de vCenter Server Appliance 8.x**  
   - Montar ISO del VCSA, ejecutar `vcsa-ui-installer` → `installer.exe` (Windows), seguir asistente (Stage 1 & 2), usar **IP/FQDN estáticos** y **DNS** resolviendo **A/PTR**. [1](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/Diploma%20EXPERTO%20EN%20VIRTUALIZACI%C3%93N.pdf)

> En el documento del proyecto encontrarás un paso a paso con capturas de **creación de datastore**, y la instalación de **VCSA** con la selección de tamaño, red y contraseñas. [1](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/Diploma%20EXPERTO%20EN%20VIRTUALIZACI%C3%93N.pdf)

---

## 🌐 Red y almacenamiento

- **vSwitch0 (gestión)** y **portgroup Management** para ESXi & vCenter.  
- **vSphere Distributed Switch (vDS)** para tráfico de VMs y servicios (opcional en entornos multi‑host; recomendado en producción). [2](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/_layouts/15/Doc.aspx?sourcedoc=%7BB1F85076-CB91-4ECF-9328-DDBA0BADF030%7D&file=POR%20CONSOLA%20SSH%20-%20Y%20VMTOOLS.docx&action=default&mobileredirect=true)[3](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/Laboratorio_%20Gesti%C3%B3n%20de%20VMs%20por%20SSH%20en%20ESXi.pdf)  
- **Datastore VMFS6** sobre disco adicional (nested) creado con **partedUtil/vmkfstools** vía **SSH**. Ver comandos más abajo. [1](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/Diploma%20EXPERTO%20EN%20VIRTUALIZACI%C3%93N.pdf)

> La guía del Módulo 2 documenta diferencias **vSS vs vDS**, **VLANs (0,1–4094, 4095)**, y **NIC Teaming** (port‑ID, IP‑hash, failover). [2](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/_layouts/15/Doc.aspx?sourcedoc=%7BB1F85076-CB91-4ECF-9328-DDBA0BADF030%7D&file=POR%20CONSOLA%20SSH%20-%20Y%20VMTOOLS.docx&action=default&mobileredirect=true)[3](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/Laboratorio_%20Gesti%C3%B3n%20de%20VMs%20por%20SSH%20en%20ESXi.pdf)

---

## 🔧 Gestión por SSH/ESXi Shell (comandos listos)

> Habilita SSH (Host → Actions → Services → **Enable SSH**) y conéctate (`ssh root@ip_esxi`). [1](https://gmqualitytechnologysl365-my.sharepoint.com/personal/adrian_jibmac_digitechfp_com/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/Diploma%20EXPERTO%20EN%20VIRTUALIZACI%C3%93N.pdf)

### 1) Listar VMs y estado
```sh
vim-cmd vmsvc/getallvms
vim-cmd vmsvc/get.summary <vmid>
vim-cmd vmsvc/power.getstate <vmid>
