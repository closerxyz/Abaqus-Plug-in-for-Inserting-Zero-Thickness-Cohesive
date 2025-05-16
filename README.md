# Abaqus-Plug-in-for-Inserting-Zero-Thickness-Cohesive


Detailed introduction and video:
        https://abaquscohesiveelementplugin.quora.com/An-ABAQUS-Plug-in-for-Generating-Cohesive-Elements
        

# ABAQUS Cohesive Element Insertion Plugin V3.1  
## User Manual  

---

## Table of Contents
1. [Plugin Introduction](#1-plugin-introduction)  
2. [Main Features and Functions](#2-main-features-and-functions)  
3. [Plugin Installation and Usage](#3-plugin-installation-and-usage)  
   3.1 [Plugin Installation](#31-plugin-installation)  
   3.2 [Plugin Interface](#32-plugin-interface)  
   3.3 [Plugin Usage Methods](#33-plugin-usage-methods)  
      - 3.3.1 [Basic Usage](#331-basic-usage)  
      - 3.3.2 [Global Cohesive Element Insertion](#332-global-cohesive-element-insertion)  
      - 3.3.3 [Insertion of Cohesive Elements with Thickness](#333-insertion-of-cohesive-elements-with-thickness)  
      - 3.3.4 [Special Set Function](#334-special-set-function)  
      - 3.3.5 [Creating Cohesive Sets After Insertion](#335-creating-cohesive-sets-after-insertion)  
   3.4 [Notes and Common Issues](#34-notes-and-common-issues)  
4. [Plugin Efficiency Testing](#4-plugin-efficiency-testing)  
5. [Application Examples](#5-application-examples)  
   5.1 [Cohesive Element Insertion in Concrete Mesoscale Models](#51-cohesive-element-insertion-in-concrete-mesoscale-models)  
   5.2 [Cohesive Element Insertion at Interfaces in Voxel Meshes](#52-cohesive-element-insertion-at-interfaces-in-voxel-meshes)  
   5.3 [Insertion of Cohesive Elements with Thickness in 3D Meshes](#53-insertion-of-cohesive-elements-with-thickness-in-3d-meshes)  
   5.4 [Hydraulic Fracturing Simulation](#54-hydraulic-fracturing-simulation)  

---

## 1. Plugin Introduction  
Cohesive Insertion is a Python-based ABAQUS plugin designed to embed cohesive elements within finite element solid meshes. It simulates complex mechanical behaviors such as multi-crack initiation and propagation in materials or structures. The plugin enables global or local insertion of zero-thickness/thickness cohesive elements in existing 2D/3D finite element meshes, significantly reducing modeling difficulty and improving efficiency. It is particularly valuable for numerical simulations in composite materials research.  

**Supported ABAQUS Versions:** 6.14 to 2023.  

---

## 2. Main Features and Functions  
1. Supports embedding cohesive elements between 2D (triangles, quadrilaterals) and 3D (tetrahedrons, wedges, hexahedrons) solid elements. Mixed element types are allowed.  
2. Flexible insertion locations: global (all shared faces), local (specific regions), or at matrix-inclusion interfaces.  
3. Supports zero-thickness or thickness-specified cohesive elements at interfaces.  
4. Enables pore-pressure cohesive elements for hydraulic fracturing simulations.  
5. Allows creating separate sets for cohesive elements in specified geometric regions (e.g., rock joints, crystal grain boundaries).  
6. Automatic cohesive set creation:  
   - Internal cohesive elements are grouped as "SetName-cohesive" (e.g., "Set-1-cohesive").  
   - Interface cohesive elements are named "Set1-by-Set2-cohesive" (e.g., "Set-1-by-Set-2-cohesive").  
7. Post-insertion cohesive set creation based on solid element relationships.  
8. High-efficiency insertion algorithms with minimal computational overhead.  

---

## 3. Plugin Installation and Usage  
### 3.1 Plugin Installation  
1. Extract the plugin package to a folder.  
2. Copy the folder `Cohesive Insertion (V3.1)` to one of these paths (choose only one):  
   - ABAQUS plugin installation path (e.g., for 2022: `C:\Users\[User]\abaqus_plugins`).  
   - ABAQUS default plugin path (e.g., `C:\SIMULIA\CAE\2022\win_b64\code\plugins`).  
3. Restart ABAQUS. The plugin will appear under `Plug-ins > Cohesive Insertion V3.1`.  

### 3.2 Plugin Interface  
The plugin interface includes:  
- **New Part Block**: Configure the new part containing inserted cohesive elements.  
  - `Name`: New part name.  
  - `Model/Part`: Original model and part.  
- **Matrix Block**: Define the matrix set and whether to insert cohesive elements inside it.  
- **Inclusion Block**: Define inclusion sets and insertion options.  
- **Special Set**: Assign cohesive sets to specific edges (2D) or faces (3D).  
- **Element Options**:  
  - `Specify Interface Thickness`: Enable thickness for cohesive elements.  
  - `Thickness`: Set element thickness.  
  - `Remesh Matrix`: Improve mesh quality for thickness insertion (requires tri/tet elements).  
  - `Add Pore Pressure Nodes`: Enable pore-pressure cohesive elements.  
- **Create Cohesive Set After Insertion**: Group cohesive elements post-insertion.  

Click `OK` to run. The plugin generates a new part and reports insertion statistics.  

### 3.3 Plugin Usage Methods  
#### 3.3.1 Basic Usage  
1. Create a 2D/3D part with defined sets (e.g., matrix `A`, inclusions `B` and `C`).  
2. Mesh the part.  
3. Configure the plugin:  
   - Select sets for matrix/inclusions.  
   - Choose insertion locations (internal/interface).  
4. Generated cohesive sets follow naming conventions (e.g., `A-cohesive`, `A-by-B-cohesive`).  

#### 3.3.2 Global Cohesive Element Insertion  
- Set the entire part as one set (e.g., `ALL`).  
- Enable `Insert Element Inside` for global insertion.  

#### 3.3.3 Insertion of Cohesive Elements with Thickness  
- Only applicable at matrix-inclusion interfaces (not internally).  
- Specify thickness and optionally enable `Remesh Matrix`.  

#### 3.3.4 Special Set Function  
- Assign cohesive sets to predefined edges/faces (e.g., weak interfaces).  
- Example: Global insertion with a separate set for `Weak` edges.  

#### 3.3.5 Creating Cohesive Sets After Insertion  
- Refine cohesive sets post-insertion using solid set relationships.  
- Rules:  
  - `Set1=A, Set2=B` → `A-by-B-cohesive` (interface).  
  - `Set1=A, Set2=empty` → `A-cohesive` (internal).  

### 3.4 Notes and Common Issues  
1. The plugin works on a single part. Merge multiple parts via the Assembly module if needed.  
2. The part must be meshed with consecutive node/element numbering starting from 1.  
3. Use linear elements only (2 nodes per edge).  
4. Avoid reinserting cohesive elements into the same part.  

---

## 4. Plugin Efficiency Testing  
| 3D Tetrahedral Mesh Size | Cohesive Elements Inserted | Time (s) |  
|--------------------------|----------------------------|----------|  
| 6,585                    | 12,570                     | 0.466    |  
| 40,359                   | 78,318                     | 2.448    |  
| 212,573                  | 415,546                    | 13.106   |  
| 466,557                  | 914,298                    | 29.447   |  
| 818,762                  | 1,607,278                  | 52.269   |  
| 1,673,524                | 3,287,048                  | 110.553  |  

---

## 5. Application Examples  
### 5.1 Concrete Mesoscale Models  
Cohesive elements inserted between aggregate and mortar phases.  

### 5.2 Voxel Mesh Interfaces  
Zero-thickness cohesive elements at aggregate-mortar interfaces.  

### 5.3 3D Meshes with Thickness  
Thickness-specified cohesive elements at material interfaces.  

### 5.4 Hydraulic Fracturing Simulation  
Pore-pressure cohesive elements for fracture propagation analysis.  
