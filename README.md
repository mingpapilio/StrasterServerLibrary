# Straster Server Library

This repository contains user manuals, maintenance notes, and related documentation for the shared StrasterTower and FosterStorage server ecosystem.

The aim is to provide a simple, central, and easy-to-maintain documentation library for users and administrators across the Foster and Stracy labs.

## Documentations
### StrasterTower
StrasterTower is the main computation and processing server. Users normally connect to StrasterTower through SSH and use it for analysis, simulations, JupyterLab, and server-side workflows.
- [StrasterTower User Manual](StrasterTower/UserManual_v2.1.md)

### FosterStorage
FosterStorage is the shared storage server. It is intended for storing and organising large datasets, shared lab files, and data exchanged between machines.
- [FosterStorage User Manual](FosterStorage/UserManual_v1.md)

### Maintenance
This section contains admin-facing notes for server setup, maintenance, troubleshooting, and future updates.

- [Maintenance Notes](Maintenance/MaintenanceNotes_v1.md)

## Repository structure
```text
StrasterServerLibrary/
├── FosterStorage/
│   ├── preview.png
│   └── UserManual_v1.md
├── Maintenance/
│   └── MaintenanceNotes_v1.md
├── StrasterTower/
│   ├── images/
│   └── UserManual_v2.1.md
├── LICENSE
└── README.md