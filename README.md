# 🏭 Taller IIoT Industrial — Arduino Day LATAM 2026

**De los sensores a la nube: automatización industrial con Node-RED, MQTT y LOGO 8.4**

---

## ¿De qué trata el taller?

Un recorrido práctico por la capa de automatización industrial: desde el PLC hasta el dashboard en tiempo real, pasando por MQTT como protocolo de mensajería IIoT. El taller simula un variador de frecuencia controlando una máquina real, con visualización de estados, monitoreo de velocidad y comunicación entre capas.

```
PLC Siemens LOGO 8.4
        │
        │ señales digitales
        ▼
  Node-RED (lógica)
        │
        │ MQTT  ──────────────────── Broker EMQX
        ▼                                   │
  Dashboard 2.0 (HMI)          otros clientes MQTT
        │
        ▼
  State Inspector · Insight · Velocidad
```

---

## Contenido del repositorio

```
├── flows/
│   ├── flow_comunicacion.json      # Capa MQTT — comandos y señales al PLC
│   ├── flow_industrial_iioT.json   # Dashboard HMI — controles, estados, lógica
│   └── flow_insight.json           # Monitoreo en tiempo real con Insight
├── Programa LOGO 8.4/
│   └── Logica_control.lsc           # Programa LOGO para Siemens LOGO 8.4
├── assets/
│   ├── velocidad_maquina.html      # Dashboard HTML standalone de velocidad
│   └── tabla observacion.docx      # Tabla de observación para los participantes
└── Presentacion/
    └── presentacionTaller.pdf      # Presentación del taller (10 páginas)
```

---

## Requisitos

| Herramienta | Versión | Notas |
|---|---|---|
| Node-RED | ≥ 3.0 | [nodered.org](https://nodered.org) |
| @flowfuse/node-red-dashboard | ≥ 1.0 | Palette Manager |
| node-red-contrib-device-simulator-energiam | última | Palette Manager |
| node-red-dashboard-2-state-inspector-energiam | última | Palette Manager |
| EMQX MQTT Broker local | ≥ 2.0 | local o en red |
| LOGO! Soft Comfort | V8.4 | para el programa PLC (opcional) |
| Node.js | ≥ 18 | |

---

## Instalación rápida

### 1. Clonar el repositorio

```bash
git clone https://github.com/energiamEcoTouch/arduino-day-latam-2026.git
cd arduino-day-latam-2026
```

### 2. Instalar nodos desde Palette Manager

En Node-RED: **Menú → Manage Palette → Install**

```
@flowfuse/node-red-dashboard
node-red-contrib-device-simulator-energiam
node-red-dashboard-2-state-inspector-energiam
```

### 3. Importar los flows

**Menú → Import** → seleccionar los tres archivos de la carpeta `flows/` en este orden:

1. `flow_comunicacion.json`
2. `flow_industrial_iioT.json`
3. `flow_insight.json`

### 4. Configurar el broker MQTT

En los nodos MQTT, cambiar la IP del broker por la de tu red:

```
Broker: 192.168.1.55  →  <tu IP>
Puerto: 1883
```

### 5. Deploy y abrir el dashboard

```
http://localhost:1880/dashboard
```

---

## Los tres flows explicados

### `flow_comunicacion.json` — Capa de transporte

Publica y suscribe comandos vía MQTT hacia el PLC y otros clientes.

| Tópico MQTT | Descripción |
|---|---|
| `hab` | Habilitar / inhabilitar el driver |
| `start` | Arrancar la máquina |
| `stop` | Detener la máquina |
| `stopE` | Parada de emergencia |
| `jog` | Velocidad en modo jog |

---

### `flow_industrial_iioT.json` — HMI + lógica

Dashboard completo con controles, visualización de estados según **IEC 60073** y lógica de secuencias.

**Paneles del dashboard:**
- Switches de habilitación
- Botones START / STOP / JOG / Reset
- Slider de velocidad objetivo
- Torre de LEDs (estado de la máquina)
- State Inspector — tabla comparativa de variables por estado
- CPU / red — métricas del sistema

---

### `flow_insight.json` — Monitoreo en tiempo real

Visualización histórica y en tiempo real de la velocidad de la máquina usando **Node-RED Insight** y el simulador de dispositivos EnergIAM.

- Simulación de un variador acelerando, en régimen y desacelerando
- Visualización por tabs: última hora, día, semana
- Exportable a CSV

---

## Programa LOGO 8.4

El archivo `Logica_control.lsc` es un programa para **Siemens LOGO 8.4** que controla la logica de la maquina simulada e integra señales digitales con el sistema Node-RED vía Ethernet.

Para abrirlo: **LOGO! Soft Comfort V8.4** → Archivo → Abrir.

---

## Arquitectura del taller

```
┌─────────────────────────────────────────────────────┐
│                   Dashboard 2.0                     │
│  HMI │ State Inspector │ Insight │ Velocidad HTML   │
└──────────────────────┬──────────────────────────────┘
                       │ socket / HTTP
┌──────────────────────┴──────────────────────────────┐
│                    Node-RED                         │
│  flow_comunicacion │ flow_industrial │ flow_insight │
└──────────────────────┬──────────────────────────────┘
                       │ MQTT
              ┌────────┴────────┐
              │  Broker MQTT    │
              │  (EMQX)    │
              └────────┬────────┘
                       │ Ethernet
              ┌────────┴────────┐
              │ LOGO 8.4 / PLC  │
              └─────────────────┘
```

---

## Sobre el taller

Este taller forma parte de la iniciativa **Arduino Day LATAM 2026**, un evento global de la comunidad Arduino. El contenido está orientado a profesionales y estudiantes de automatización, electricidad e ingeniería que quieran dar el salto al IIoT industrial sin abandonar los fundamentos de la automatización tradicional.

**Duración:** 1,5 horas  
**Modalidad:** Presencial / Virtual  
**Nivel:** Intermedio  
**Idioma:** Español

---

## Autor

**Adrian Iskow** — [EnergIAM EcoTouch](https://github.com/energiamEcoTouch)

---

## Licencia

MIT © 2026 EnergIAM EcoTouch

---

<p align="center">
  Hecho con ☕ para Arduino Day LATAM 2026
</p>
