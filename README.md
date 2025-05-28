# Abaqus-Plug-in-for-Inserting-Zero-Thickness-Cohesive

# Notes：Cohesive Insertion V3.1 for ABAQUS 2023 and older；Cohesive Inerstion V4.1 for ABAQUS 2024 and later

Detailed introduction and video:
        https://abaquscohesiveelementplugin.quora.com/An-ABAQUS-Plug-in-for-Generating-Cohesive-Elements
        
# ABAQUS Cohesive Element Insertion Plugin V3.1  
## User Manual  

![Plugin Logo](media/image3.png "Cohesive Insertion Plugin Logo")  

---

## Table of Contents
1. [Plugin Introduction](#1-plugin-introduction)  
2. [Main Features](#2-main-features)  
3. [Installation & Usage](#3-installation--usage)  
   3.1 [Installation Guide](#31-installation-guide)  
   3.2 [Interface Overview](#32-interface-overview)  
   3.3 [Usage Methods](#33-usage-methods)  
      - 3.3.1 [Basic Operations](#331-basic-operations)  
      - 3.3.2 [Global Insertion](#332-global-insertion)  
      - 3.3.3 [Thick Elements](#333-thick-elements)  
      - 3.3.4 [Special Sets](#334-special-sets)  
      - 3.3.5 [Post-Insertion Sets](#335-post-insertion-sets)  
   3.4 [Troubleshooting](#34-troubleshooting)  
4. [Performance Benchmarks](#4-performance-benchmarks)  
5. [Case Studies](#5-case-studies)  
   5.1 [Concrete Modeling](#51-concrete-modeling)  
   5.2 [Voxel Meshes](#52-voxel-meshes)  
   5.3 [3D Thick Elements](#53-3d-thick-elements)  
   5.4 [Hydraulic Fracturing](#54-hydraulic-fracturing)  

---

## 1. Plugin Introduction  
Cohesive Insertion is a Python-based ABAQUS plugin for embedding cohesive elements within solid element meshes to simulate crack initiation/propagation.  

**Compatibility**: ABAQUS 6.14 through 2023  

![Installation Path Example](media/image4.png "Typical plugin installation directory")  

---

## 2. Main Features  
### Core Capabilities:  
- **Multi-element Support**:  
  - 2D: Triangles/Quadrilaterals  
  - 3D: Tetrahedrons/Wedges/Hexahedrons  
- **Insertion Modes**:  
  - Global (all shared faces)  
  - Local (selected regions)  
  - Matrix-Inclusion interfaces  
- **Element Types**:  
  - Zero-thickness  
  - User-defined thickness  
  - Pore-pressure enabled  

### Advanced Functions:  
1. Automatic set naming:  
   - Internal: `SetName-cohesive`  
   - Interface: `Set1-by-Set2-cohesive`  
2. Special geometric set assignment  
3. Post-insertion set creation  

![Plugin Interface](media/image7.png "Main plugin control panel")  

---

## 3. Installation & Usage  
### 3.1 Installation Guide  
**Step-by-Step**:  
1. Extract package → Locate `Cohesive Insertion (V3.1)` folder  
2. Copy to either:  
   - User plugin directory  
   - ABAQUS default plugin path  
3. Restart ABAQUS → Access via `Plug-ins` menu  

> ⚠️ **Critical Note**: Never install in both locations simultaneously  

![Plugin Menu Location](media/image6.png "ABAQUS plugin menu entry")  

### 3.2 Interface Overview  
**Control Blocks**:  

| Block | Parameters |  
|-------|------------|  
| **New Part** | Name, Model, Base Part |  
| **Matrix** | Set, Insert Inside toggle |  
| **Inclusion** | Multiple sets with insertion toggles |  
| **Special Set** | Edge/Face geometric sets |  
| **Element Options** | Thickness, Remesh, Pore Pressure |  

**Output Example**:  
![Result Notification](media/image8.png "Successful insertion message")  

### 3.3 Usage Methods  
#### 3.3.1 Basic Operations  
**Workflow**:  
1. Create part with defined sets (e.g., matrix A, inclusions B/C)  
2. Generate mesh  
3. Configure plugin:  
   ```markdown
   - Matrix: Set A (Insert Inside ✔)  
   - Inclusion: Set B (Insert Inside ✔), Set C (Insert Inside ✖)  
