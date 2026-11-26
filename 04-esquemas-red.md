# 04. Esquemas de Red, Mapeo de Puertos y Flujo de la Señal

## 1. Diagrama de Conexiones Físicas

La topología de la maqueta implementa un enlace óptico punto a punto bidireccional sobre dos fibras monomodo independientes (sentido **A $ightarrow$ B** y sentido **B $ightarrow$ A**), centralizando las interconexiones troncales en el **ODF** e integrando puntos de inserción para el **OVA** y monitoreo espectral mediante el **OSA**.

![Diagrama de Topología Física DWDM](../diagramas/imagenes/topologia-dwdm.png)

> **Archivos fuente editables:** El diagrama editable en formato Draw.io se encuentra respaldado en [`diagramas/esquemas-drawio/topologia-dwdm.drawio`](../diagramas/esquemas-drawio/topologia-dwdm.drawio) y su versión vectorial en [`diagramas/imagenes/topologia-dwdm.svg`](../diagramas/imagenes/topologia-dwdm.svg).

---

## 2. Mapeo de Puertos (Origen TX $ightarrow$ Destino RX)

La siguiente tabla detalla la interconexión física punto a punto entre todos los equipos del rack (considerando enlaces dúplex con fibras separadas para transmisión y recepción):

| Tramo | Equipo Origen (Puerto TX) | Medio / Conector | Equipo Destino (Puerto RX) | Observación técnica |
| :---: | :--- | :--- | :--- | :--- |
| **1** | **MTX150x** (Port 1 TX — SFP/SFP+) | Patch cord LC-LC (MM/SM) | **Switch CSS610** (SFP+ 1 RX) | Inyección de tráfico de prueba L2/L3 |
| **2** | **Switch CSS610** (SFP+ 2 TX) | Patch cord LC-LC | **HT6000 #1** (OTU Client 1 RX) | Tráfico de cliente agregado hacia transpondedor |
| **3** | **HT6000 #1** (OTU Line 1 TX — $\lambda_n$) | Patch cord LC-LC SM | **HT6000 #1** (MUX Ch $n$ IN) | Señal coloreada en Banda C (ITU-T G.694.1) |
| **4** | **HT6000 #1** (MUX LINE OUT) | Patch cord LC-LC SM | **ODF** (Puerto Panel A1 — Cara trasera/frontal) | Salida compuesta multiplexada sentido A $ightarrow$ B |
| **5** | **ODF** (Puerto Panel A1) | Fibra monomodo G.652 | **Carrete Fibra AB - 25 KM** (IN) | Ingreso a bobina de simulación de 25 km |
| **6** | **Carrete Fibra AB - 25 KM** (OUT) | Patch cord LC-LC SM | **ODF** (Puerto Panel B1) | Retorno del tramo de 25 km al panel de parcheo |
| **7** | **ODF** (Puerto Panel B1) | Patch cord LC-LC SM | **OVA JW3303** (IN) | Inserción de atenuador óptico variable en serie |
| **8** | **OVA JW3303** (OUT) | Patch cord LC-LC SM | **HT6000 #2** (DEMUX LINE IN) | Ingreso de señal atenuada al nodo receptor B |
| **9** | **HT6000 #2** (DEMUX MON) | Patch cord LC SM | **OSA RXT4510** (Optical IN) | Derivación del 1% (-20 dB) para análisis espectral |
| **10** | **HT6000 #2** (DEMUX Ch $n$ OUT) | Patch cord LC-LC SM | **HT6000 #2** (OTU Line 1 RX) | Canal óptico demultiplexado hacia receptor OTU |
| **11** | **HT6000 #2** (OTU Client 1 TX) | Patch cord LC-LC (Loop) | **HT6000 #2** (OTU Client 1 RX) | Bucle físico remoto para retorno de prueba |
| **12** | **HT6000 #2** (OTU Line 1 TX — $\lambda_n$) | Patch cord LC-LC SM | **HT6000 #2** (MUX Ch $n$ IN) | Retorno de señal hacia Nodo A |
| **13** | **HT6000 #2** (MUX LINE OUT) | Patch cord LC-LC SM | **ODF** (Puerto Panel B2) | Salida compuesta multiplexada sentido B $ightarrow$ A |
| **14** | **ODF** (Puerto Panel B2) | Fibra monomodo G.652 | **Carrete Fibra BA - 25 KM** (IN) | Ingreso a bobina de retorno de 25 km |
| **15** | **Carrete Fibra BA - 25 KM** (OUT) | Patch cord LC-LC SM | **HT6000 #1** (DEMUX LINE IN) | Recepción en Nodo A (vía ODF Puerto A2) |
| **16** | **HT6000 #1** (DEMUX Ch $n$ OUT $ightarrow$ OTU $ightarrow$ CSS610) | Patch cord LC-LC | **MTX150x** (Port 1 RX) | Cierre de lazo extremo a extremo para cálculo de BER |

---

## 3. Flujo de la Señal Extremo a Extremo

1. **Generación y agregación eléctrica/óptica de cliente (MTX150x $ightarrow$ CSS610):**
   El analizador Ethernet **MTX150x** genera tramas de prueba (RFC 2544 / Y.1564) a 1 Gbps o 10 Gbps y las entrega al **Switch MikroTik CSS610**, el cual conmuta el flujo hacia el puerto `SFP+` de enlace ascendente conectado a la tarjeta transpondedora del primer nodo.
2. **Conversión O-E-O y adaptación OTN en Nodo A (Chasis HT6000 #1):**
   El puerto de cliente de la tarjeta **OTU** recibe la señal óptica gris (ej. 850 nm o 1310 nm), la convierte al dominio eléctrico, encapsula las tramas Ethernet dentro de una trama **OTN (ITU-T G.709)** añadiendo bytes de redundancia **FEC**, y modula un láser de Banda C sintonizado en una frecuencia exacta de la grilla **ITU-T G.694.1** ($\lambda_n$).
3. **Multiplexación óptica (MUX HT6000 #1):**
   La portadora óptica coloreada ingresa al puerto correspondiente del módulo **MUX**, donde se combina pasivamente con otras longitudes de onda hacia un único puerto de salida compuesto (`LINE OUT`).
4. **Transporte troncal, simulación de distancia y atenuación (ODF $ightarrow$ Carrete AB $ightarrow$ OVA):**
   A través del **ODF**, la señal compuesta atraviesa el **Carrete de Fibra AB - 25 KM**, sufriendo una atenuación física aproximada de $5	ext{ dB}$ y acumulación de dispersión cromática. Posteriormente pasa por el **Atenuador Óptico Variable (JW3303)**, donde se introduce pérdida controlada para evaluar el margen del enlace.
5. **Monitoreo espectral y demultiplexación en Nodo B (Chasis HT6000 #2 + OSA RXT4510):**
   En el extremo remoto, una fracción calibrada de la señal se deriva desde el puerto `MON` hacia el **OSA RXT4510** para medir potencia por canal y **OSNR**. Simultáneamente, el módulo **DEMUX** separa la longitud de onda $\lambda_n$ y la entrega al receptor óptico de línea de la tarjeta **OTU** del **HT6000 #2**, donde se realiza la detección O/E y corrección de errores FEC.
6. **Retorno en bucle y evaluación de calidad (Loopback $ightarrow$ Carrete BA $ightarrow$ MTX150x):**
   La señal recuperada en el puerto de cliente del **HT6000 #2** se reinyecta mediante un *loopback* físico hacia su propio transmisor, recorriendo el camino inverso a través del **Carrete de Fibra BA - 25 KM** y el **Switch CSS610** hasta alcanzar el receptor del **MTX150x**, que compara las tramas recibidas contra las transmitidas para determinar la **pérdida de tramas** y el **BER** efectivo.
