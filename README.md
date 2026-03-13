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
