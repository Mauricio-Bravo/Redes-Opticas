# 01. Fundamentos Teóricos de la Tecnología DWDM

## 1. Definición de DWDM y comparación con CWDM

La **Multiplexación por División de Longitud de Onda Densa (DWDM, *Dense Wavelength Division Multiplexing*)** es una tecnología de transporte óptico que permite transmitir simultáneamente múltiples portadoras ópticas independientes (cada una con su propia longitud de onda $\lambda$ o frecuencia central $f$) a través de una misma fibra óptica monomodo.

A diferencia de **CWDM** (*Coarse Wavelength Division Multiplexing*), que utiliza canales muy separados entre sí (típicamente 20 nm) a lo largo de un amplio rango espectral (1270 nm a 1610 nm) con láseres no refrigerados de bajo costo, **DWDM** concentra decenas de canales con un espaciamiento muy estrecho dentro de una banda específica (principalmente la Banda C), lo que permite amplificación óptica conjunta mediante **EDFA** (*Erbium-Doped Fiber Amplifier*) y alcances de larga distancia.

| Característica | CWDM (ITU-T G.694.2) | DWDM (ITU-T G.694.1) |
| :--- | :--- | :--- |
| **Espaciamiento entre canales** | 20 nm ($pprox 2500	ext{ GHz}$) | 100 GHz ($pprox 0.8	ext{ nm}$), 50 GHz ($pprox 0.4	ext{ nm}$), 25 GHz o *Flex-Grid* |
| **Rango espectral típico** | 1270 nm – 1610 nm (Bandas O, E, S, C, L) | 1530 nm – 1565 nm (Banda C) / 1565 nm – 1625 nm (Banda L) |
| **Capacidad de canales** | Hasta 18 canales (16 habituales sin pico de agua) | 40, 80, 96 o más canales por par de fibra |
| **Control térmico de láser** | No refrigerado (permite deriva térmica) | Refrigerado mediante controlador Peltier (TEC) para estabilizar $\lambda$ |
| **Amplificación óptica** | No compatible con EDFA convencional | Compatible con amplificadores EDFA en Banda C y Banda L |
| **Alcance y aplicación** | Corto/medio alcance (metro/acceso, hasta ~80 km sin regeneración) | Largo alcance y ultra-largo alcance (*core*, *metro-core*, submarino) |

---

## 2. Espectro óptico, Banda C y Grilla Espectral (ITU-T G.694.1)

### Operación en Banda C (*Conventional Band*)
Los sistemas DWDM operan prioritariamente en la **Banda C (1530 nm a 1565 nm)** debido a dos propiedades físicas fundamentales de la fibra de sílice:
1. **Ventana de mínima atenuación:** Alrededor de los **1550 nm**, la fibra monomodo presenta su menor pérdida intrínseca por dispersión de Rayleigh y absorción infrarroja ($pprox 0.18	ext{ a }0.22	ext{ dB/km}$).
2. **Ganancia de amplificadores EDFA:** La fibra dopada con iones de erbio amplifica ópticamente todas las longitudes de onda dentro de la Banda C de forma simultánea sin requerir conversión óptico-eléctrico-óptica (O-E-O).

### Grilla Espectral ITU-T G.694.1
La recomendación **ITU-T G.694.1** define la grilla de frecuencias nominales centrales anclada a una frecuencia de referencia de **$193.1	ext{ THz}$** ($pprox 1552.52	ext{ nm}$ en el vacío):

$$f_n = 193.1	ext{ THz} + n 	imes \Delta f$$

Donde $n \in \mathbb{Z}$ (positivo, cero o negativo) y $\Delta f$ es el espaciamiento de canal:
- **Grilla fija de 100 GHz ($pprox 0.8	ext{ nm}$):** Permite alrededor de 40 a 48 canales en Banda C.
- **Grilla fija de 50 GHz ($pprox 0.4	ext{ nm}$):** Duplica la densidad espectral permitiendo 80 a 96 canales en Banda C.
- **Grilla Flexible (*Flex-Grid*):** Define ranuras (*frequency slots*) con granularidad de frecuencia central de **$6.25	ext{ GHz}$** y anchos espectrales múltiplos de **$12.5	ext{ GHz}$**, permitiendo acomodar supercanales coherentes de 200G, 400G u 800G sin desperdiciar espectro.

---

## 3. Modulación Óptica y Conversión Electro-Óptica

### Conversión Electro-Óptica (E/O y O/E)
En un sistema DWDM, los transpondedores o *muxponders* realizan la adaptación entre las interfaces de cliente (señales de corto alcance, por ejemplo 850 nm o 1310 nm) y las interfaces de línea DWDM (longitudes de onda coloreadas de la grilla ITU-T):
- **Transmisión (E/O):** La señal óptica de cliente se convierte a dominio eléctrico (O/E), se procesa (encapsulación OTN y corrección de errores FEC) y modula una fuente láser sintonizable de Banda C mediante moduladores directos (DML) o externos como **Mach-Zehnder (MZM)**, que varían la intensidad, fase o polarización de la luz.
- **Recepción (O/E):** Un fotodetector (**PIN** o fotodiodo de avalancha **APD**) en receptores de detección directa, o un receptor coherente con oscilador local (LO) y mezclador óptico de $90^\circ$, convierte la portadora óptica nuevamente en señal eléctrica para su decodificación.

### Formatos de Modulación Óptica

- **NRZ (*Non-Return-to-Zero*):** Modulación binaria de intensidad (OOK, *On-Off Keying*) donde un nivel alto de potencia óptica representa un `1` lógico y un nivel bajo representa un `0` lógico (1 bit por símbolo). Es el estándar en enlaces de **10 Gbps** por su simplicidad y robustez, aunque presenta menor eficiencia espectral.
- **PAM4 (*Pulse Amplitude Modulation 4-level*):** Codifica **2 bits por símbolo** utilizando cuatro niveles distintos de amplitud óptica (`00`, `01`, `10`, `11`). Duplica la tasa binaria para un mismo ancho de banda eléctrico/óptico (usado en interfaces de **50G, 100G y 400G** de corto/medio alcance), a costa de requerir una relación señal-ruido (OSNR) mayor debido a la menor separación entre niveles.
- **QPSK / DP-QPSK (*Dual-Polarization Quadrature Phase Shift Keying*):** Modulación coherente que codifica información en **cuatro estados de fase** ($45^\circ, 135^\circ, 225^\circ, 315^\circ$, equivalente a 2 bits por símbolo) sobre dos polarizaciones ortogonales (X e Y), logrando **4 bits por símbolo**. Es el estándar de transporte coherente a **100 Gbps** por su alta tolerancia a la dispersión cromática (CD) y dispersión por modo de polarización (PMD).
- **QAM (*Quadrature Amplitude Modulation*, ej. 16-QAM, 64-QAM):** Combina modulación de amplitud y fase en ambas polarizaciones (DP-16QAM = 8 bits/símbolo; DP-64QAM = 12 bits/símbolo). Permite transmisiones de **200G, 400G y 800G+** por longitud de onda, exigiendo altos niveles de OSNR y procesamiento digital de señales (DSP) avanzado.

---

## 4. Estándares y Normativas ITU-T

1. **ITU-T G.694.1 (*Spectral grids for WDM applications: DWDM frequency grid*):**
   Establece las frecuencias nominales centrales de referencia (ancladas en $193.1	ext{ THz}$), los espaciamientos estándar (12.5 GHz, 25 GHz, 50 GHz, 100 GHz) y la arquitectura de grilla flexible (*Flex-Grid*) que garantiza la interoperabilidad entre fabricantes de equipos ópticos.
2. **ITU-T G.709 (*Interfaces for the Optical Transport Network - OTN*):**
   Define la jerarquía de transporte digital (contenedor digital óptico *Optical Digital Wrapper*: OPUk, ODUk, OTUk) que encapsula servicios clientes (Ethernet, SDH/SONET, Fibre Channel) de forma transparente. Incorpora cabecera de operación, administración y mantenimiento (OAM) por canal óptico y **FEC (*Forward Error Correction*)**, mejorando drásticamente el presupuesto óptico y el alcance antes de requerir regeneración.
3. **ITU-T G.652 (*Characteristics of a single-mode optical fibre and cable*):**
   Define la fibra monomodo estándar (**SSMF**, ej. G.652.D con pico de agua suprimido). Presenta dispersión cromática nula alrededor de **1310 nm** y una dispersión cromática positiva significativa en Banda C ($pprox 17	ext{ ps/(nm}\cdot	ext{km)}$ a 1550 nm). En sistemas DWDM de alta velocidad, esta dispersión ayuda a reducir efectos no lineales (como *Four-Wave Mixing*), siendo compensada mediante módulos DCM o DSP coherente.
4. **ITU-T G.655 (*Characteristics of a non-zero dispersion-shifted single-mode optical fibre cable*):**
   Define la fibra monomodo de dispersión desplazada no nula (**NZ-DSF**). Desplaza el punto de dispersión cero fuera de la Banda C (manteniendo un valor bajo pero distinto de cero, típicamente entre $2	ext{ y }6	ext{ ps/(nm}\cdot	ext{km)}$ en 1550 nm). Esto minimiza la acumulación de dispersión cromática en enlaces de larga distancia mientras evita la mezcla de cuatro ondas (FWM) que ocurría en fibras de dispersión desplazada convencional (G.653).
