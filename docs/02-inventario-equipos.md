# 02. Inventario de Equipos de la Maqueta DWDM

A continuación se detallan las especificaciones funcionales, interfaces físicas y rol técnico de los **7 componentes** instalados en el rack de laboratorio DWDM.

---

## Resumen comparativo del equipamiento

| # | Equipo / Modelo | Categoría | Interfaces principales | Rol en la maqueta DWDM |
| :-: | :--- | :--- | :--- | :--- |
| 1 | **VeEX MTX150x** | Instrumentación Ethernet | SFP/SFP+ (1G/10G), RJ45 | Generación de tráfico L2/L3 y pruebas RFC 2544 / Y.1564 |
| 2 | **VeEX RXT4510** | Instrumentación Óptica (OSA) | Monomodo LC/APC o LC/UPC (1260–1650 nm) | Análisis espectral DWDM, potencia por canal y OSNR |
| 3 | **MikroTik CSS610-8G-2S+IN** | Conmutación Ethernet | 8x RJ45 1GbE + 2x SFP+ 10GbE | Agregación de clientes y conmutación L2 hacia el transpondedor |
| 4 | **ODF (Optical Distribution Frame)** | Infraestructura pasiva | Adaptadores LC/UPC y LC/APC | Parcheo estructurado, protección de conectores y puntos de prueba |
| 5 | **Chasis HT6000 (DWDM 1 y 2)** | Transporte Óptico DWDM | Tarjetas OTU (SFP/SFP+), MUX/DEMUX, OSC, NMS | Conversión cliente-línea, multiplexación/demultiplexación DWDM |
| 6 | **Carretes de Fibra (AB y BA)** | Medio de transmisión | Conectores LC (bobinas de 25 km c/u) | Simulación de enlace troncal bidireccional de 25 km |
| 7 | **Atenuador Óptico JW3303** | Instrumentación pasiva variable | Adaptadores FC/SC/LC monomodo | Simulación de atenuación de enlace y degradación de margen óptico |

---

## Descripción detallada por componente

### 1. Analizador Ethernet (VeEX MTX150x)
- **Función técnica:** Equipo portátil multiservicio de prueba y certificación de redes Ethernet de hasta 10 Gbps. Permite validar acuerdos de nivel de servicio (SLA), verificar conectividad de capa 2 y capa 3, inyectar tramas controladas y medir integridad de transmisión.
- **Tipo de interfaz:** Puertos ópticos **SFP / SFP+** (1000BASE-X y 10GBASE-R) y puertos eléctricos **RJ45** (10/100/1000BASE-T).
- **Rol dentro del rack:** Actúa como equipo terminal generador y receptor de tráfico de cliente (o mediante bucle físico/lógico en el extremo remoto). Ejecuta pruebas normalizadas **RFC 2544** (*throughput*, latencia, pérdida de tramas y *back-to-back*) e **ITU-T Y.1564** (*EtherSAM*) para cuantificar la tasa de error de bit (**BER**) y pérdida de paquetes al variar las condiciones del enlace óptico.

### 2. Analizador de Espectro Óptico - OSA (VeEX RXT4510)
- **Función técnica:** Módulo analizador de espectro óptico de alta resolución diseñado para redes CWDM y DWDM (con soporte de espaciamiento de 50 GHz y 100 GHz). Mide la distribución de potencia óptica en función de la longitud de onda o frecuencia.
- **Tipo de interfaz:** Puerto de entrada óptica monomodo (típicamente LC/APC o LC/UPC con adaptador intercambiable) cubriendo el rango de **1260 nm a 1650 nm** (incluyendo Banda C completa).
- **Rol dentro del rack:** Se conecta a los puertos de monitoreo (`MON`) del multiplexor/demultiplexor o mediante un acoplador (*tap*) en el ODF para inspeccionar en tiempo real los canales DWDM activos, verificar el cumplimiento de la grilla **ITU-T G.694.1**, medir potencia por canal ($	ext{dBm}$), deriva de longitud de onda ($\Delta\lambda$) y relación señal a ruido óptica (**OSNR** en $	ext{dB}$).

### 3. Switch Ethernet (MikroTik CSS610-8G-2S+IN)
- **Función técnica:** Switch gestionable de capa 2 basado en SwOS, orientado a la agregación de tráfico de acceso y transporte de enlaces ascendentes (*uplinks*) de alta velocidad con soporte de VLAN (IEEE 802.1Q) y control de flujo.
- **Tipo de interfaz:** 8 puertos eléctricos **Gigabit Ethernet (RJ45)** y 2 jaulas ópticas **SFP+ (10 Gbps)** retrocompatibles con módulos SFP (1 Gbps).
- **Rol dentro del rack:** Agrega el tráfico proveniente del analizador MTX150x o estaciones de trabajo de laboratorio hacia las interfaces SFP/SFP+ de cliente de las tarjetas transpondedoras del chasis HT6000, permitiendo multiplexar múltiples flujos Ethernet antes de su transporte óptico.

### 4. ODF (*Optical Distribution Frame*)
- **Función técnica:** Panel de distribución y terminación óptica pasiva montado en rack de 19 pulgadas que centraliza el empalme, organización de reservas de fibra y conexión mediante *patch cords*.
- **Tipo de interfaz:** Panel frontal con acopladores hembra-hembra monomodo (habitualmente **LC/UPC** azul o **LC/APC** verde).
- **Rol dentro del rack:** Actúa como punto de demarcación y parcheo ordenado entre los puertos de línea de los chasis DWDM (HT6000), los carretes de fibra de 25 km, el atenuador óptico variable (OVA) y los puertos de acceso para instrumentos de medición, evitando manipulación directa y desgaste en los transceptores ópticos.

### 5. Chasis DWDM 1 y DWDM 2 (HT6000)
- **Función técnica:** Plataforma modular de transmisión óptica DWDM de grado operador (como las series HT6000 de 1U/2U/5U) que integra tarjetas transpondedoras/muxpondedoras, multiplexores/demultiplexores ópticos pasivos (MUX/DEMUX), amplificadores ópticos (EDFA) y tarjetas de gestión.
- **Tipo de interfaz:**
  - *Lado Cliente:* Jaulas **SFP / SFP+ / QSFP28** (interfaces grises de corto alcance, 850 nm o 1310 nm).
  - *Lado Línea:* Interfaces ópticas **DWDM coloreadas** en Banda C (grilla ITU-T G.694.1) y puertos de línea compuesta (`LINE TX/RX`), puerto de monitoreo (`MON`) y canal de supervisión óptica (`OSC` a 1510 nm).
  - *Gestión:* Puerto Ethernet RJ45 (`NMS / MGMT`) y consola serie (`CONSOLE`).
- **Rol dentro del rack:** Constituyen los dos nodos terminales (Nodo A y Nodo B) de la red de transporte óptico. Convierten las señales de cliente a longitudes de onda DWDM específicas, las multiplexan sobre un único par de fibra hacia el enlace troncal y realizan el proceso inverso en el extremo remoto, gestionando alarmas ópticas y niveles de potencia.

### 6. Carretes de Fibra Óptica (AB - 25 KM y BA - 25 KM)
- **Función técnica:** Bobinas de fibra óptica monomodo estándar (**ITU-T G.652.D**) encapsuladas en cajas de laboratorio de 25 km de longitud cada una, diseñadas para emular la atenuación ($pprox 0.20	ext{ dB/km} 	imes 25	ext{ km} pprox 5	ext{ dB}$ más pérdidas de conectores) y la dispersión cromática de un tramo real de planta externa.
- **Tipo de interfaz:** Pigtails o conectores terminados en **LC/UPC** o **LC/APC** rotulados por sentido de transmisión (**AB** para el sentido Nodo 1 $
ightarrow$ Nodo 2, y **BA** para el sentido Nodo 2 $
ightarrow$ Nodo 1).
- **Rol dentro del rack:** Simulan el enlace troncal bidireccional de transporte de larga distancia entre el Chasis DWDM 1 y el Chasis DWDM 2 a través del ODF.

### 7. Atenuador Óptico Variable - OVA (JW3303)
- **Función técnica:** Instrumento óptico pasivo portátil que permite introducir una pérdida de inserción calibrada y ajustable de forma continua (típicamente en un rango de $0	ext{ a }60	ext{ dB}$ en ventanas de 1310 nm y 1550 nm) sin alterar la longitud de onda ni generar reflexiones significativas.
- **Tipo de interfaz:** Puertos ópticos monomodo de entrada y salida compatibles con conectores **FC, SC o LC** mediante adaptadores intercambiables.
- **Rol dentro del rack:** Se intercala en serie en el enlace de línea (generalmente en el ODF) para simular escenarios de envejecimiento de fibra, empalmes defectuosos o aumento de distancia, permitiendo observar el umbral de sensibilidad del receptor DWDM, la caída de OSNR en el OSA y la aparición de errores de bit (BER) en el analizador Ethernet.
