# Dell Command | Update – SCCM Deployment Case Study

## Overview
This case study documents the deployment and stabilization of Dell Command | Update (DCU)
in a Microsoft Endpoint Configuration Manager (MECM/SCCM) environment.

The objective was to improve driver, firmware, and BIOS compliance across Dell devices
while ensuring reliable, repeatable deployment behavior.

---

## Environment (Generalized)
- OS: Windows 11
- Device Types: Dell laptops and desktops (multiple models)
- Deployment Tool: SCCM / MECM
- Deployment Type: Application (Required, System context)

---

## Problem
Initial deployments of DCU failed inconsistently, reporting MSI error codes
(1602 / 1603) despite successful installation on some devices.

In several cases:
- The application installed successfully
- SCCM later reported the deployment as failed
- Reinstall attempts caused fatal MSI errors

---

## Root Causes Identified
1. **Missing prerequisite**
   - DCU requires Microsoft .NET Desktop Runtime 8.x (x64)
   - Some devices only had x86 runtime installed

2. **Unreliable detection logic**
   - MSI ProductCode detection produced false negatives
   - SCCM repeatedly attempted reinstalls of an already-installed application

---

## Resolution

### Dependency Management
- Packaged .NET Desktop Runtime 8.x (x64) as a separate SCCM Application
- Added it as an auto-install dependency to DCU

### Detection Method Correction
Replaced MSI-based detection with file-based detection:
Path: C:\Program Files\Dell\CommandUpdate
File: dcu-cli.exe
Rule: Exists
