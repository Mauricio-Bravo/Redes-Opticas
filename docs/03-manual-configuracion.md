# 03. Manual de Configuración y Procedimientos de Medición

## 1. Medidas de Seguridad Óptica

### Protección ocular frente a radiación láser (Clases 1M y 3R)
Los sistemas DWDM operan en la **Banda C ($pprox 1530	ext{ – }1565	ext{ nm}$)**, correspondiente al espectro infrarrojo cercano, el cual es **completamente invisible para el ojo humano** y no activa el reflejo natural de parpadeo.
- **Clasificación láser:** Las interfaces de transpondedores individuales suelen corresponder a **Clase 1 o 1M**, mientras que las salidas compuestas multicanal o amplificadas (EDFA) alcanzan **Clase 3R o 3B**, capaces de provocar lesiones térmicas irreversibles en la retina o la córnea.
- **Reglas obligatorias en laboratorio:**
  1. **Nunca observar directamente** el extremo de una fibra óptica, un *patch cord* ni los puertos ópticos de los equipos o del ODF, incluso si se asume que el enlace está apagado.
  2. Nunca utilizar lupas o microscopios ópticos convencionales (sin filtro de seguridad o pantalla digital) sobre fibras energizadas; la óptica convergente concentra el haz láser aumentando el riesgo ocular.
  3. Mantener **tapas protectoras** (*dust caps*) instaladas en todos los puertos y conectores que no estén en uso activo.
  4. Verificar el estado del láser mediante un **medidor de potencia óptica (OPM)** o el analizador de espectro antes de desconectar o manipular enlaces troncales.

### Limpieza de conectores y manejo de APC vs. UPC
La suciedad microscópica (polvo, grasa cutánea) en el núcleo de $9	ext{ }\mu	ext{m}$ de una fibra monomodo provoca alta atenuación, reflexiones retrógradas (*back-reflection*) y daño permanente por quemadura térmica (*pitting*) en la férula al transmitir alta potencia óptica.
- **Protocolo de limpieza:** Aplicar siempre la regla **"Inspeccionar $
ightarrow$ Limpiar $
ightarrow$ Inspeccionar"** usando limpiadores en seco tipo *one-click* (para acopladores y férulas) o toallitas libres de pelusa con alcohol isopropílico de grado óptico antes de cada acoplamiento.
- **Compatibilidad física UPC vs. APC:**
  - **UPC (*Ultra Physical Contact*, color azul):** Férula pulida plana con ligera curvatura ($0^\circ$), pérdida de retorno típica $\ge 50	ext{ dB}$.
  - **APC (*Angled Physical Contact*, color verde):** Férula pulida con un ángulo de **$8^\circ$** para desviar las reflexiones hacia el revestimiento (*cladding*), logrando una pérdida de retorno $\ge 60	ext{ dB}$.
  - **Prohibición crítica:** **Nunca acoplar un conector UPC (azul) con un conector APC (verde)**. El desajuste geométrico de $8^\circ$ deja un espacio de aire que genera pérdidas extremas y destruye físicamente ambas férulas por contacto puntual.

---

## 2. Configuración del Chasis DWDM (HT6000)

### Paso 1: Acceso y verificación inicial del chasis
1. Conectar el PC de gestión al puerto `NMS / MGMT` (RJ45) del chasis **HT6000** (asignando una dirección IP dentro de la subred de administración del equipo) o mediante cable de consola serie (`115200 bps, 8N1`).
2. Ingresar a la interfaz de gestión web (NMS) o CLI y verificar el inventario de tarjetas detectadas:
   - Tarjeta de control/gestión (`NMU`).
   - Tarjetas transpondedoras / muxpondedoras (`OTU`).
   - Módulo multiplexor/demultiplexor pasivo (`MUX/DEMUX`).
3. Confirmar que el estado térmico y de ventilación del chasis se encuentre normal sin alarmas críticas (`CRIT`).

### Paso 2: Aprovisionamiento de servicios y mapeo Cliente $\leftrightarrow$ Línea
1. Seleccionar la tarjeta **OTU** correspondiente al slot activo en el Chasis DWDM 1 y en el Chasis DWDM 2.
2. Configurar los **puertos de cliente** (ej. `Client 1` / `C1`):
   - Establecer el tipo de servicio y tasa de línea (ej. `10GE LAN` o `1000BASE-X` según el módulo SFP/SFP+ instalado).
   - Habilitar la función **ALS (*Automatic Laser Shutdown*)** si se requiere apagado automático ante corte de fibra, o deshabilitarla temporalmente durante pruebas controladas con atenuador.
   - Cambiar el estado administrativo del puerto de `Disabled` a `Enabled`.
3. Verificar la asociación interna de mapeo entre el puerto de cliente y el puerto de línea DWDM (`Line 1` / `L1`), asegurando que el encapsulado OTN (ej. `OTU2` / `OTU2e`) y el **FEC** estén habilitados de forma idéntica en ambos extremos.

### Paso 3: Asignación de longitud de onda (Canal ITU-T)
1. En la configuración del puerto de línea (`Line TX/RX`) del **Chasis DWDM 1**, seleccionar el canal DWDM según la grilla **ITU-T G.694.1** que corresponda al puerto físico del módulo **MUX/DEMUX** donde está conectado el *patch cord* (por ejemplo, Canal `C21` = $192.10	ext{ THz}$ / $1560.61	ext{ nm}$, o el canal etiquetado en el panel frontal).
2. Repetir la misma asignación de canal/frecuencia exactamente en el puerto de línea homólogo del **Chasis DWDM 2**.
3. Activar el láser de línea (`Laser Enable = ON`) en ambos nodos y comprobar que desaparezca la alarma `LOS` (*Loss of Signal*) y `LOF` (*Loss of Frame*), verificando que la potencia óptica recibida (`RX Optical Power`) se encuentre dentro del rango operativo del transceptor (típicamente entre $-1	ext{ dBm}$ y $-18	ext{ dBm}$).

---

## 3. Procedimiento de Prueba y Medición

### A. Inserción del Atenuador Óptico Variable (OVA JW3303)
1. Encender el **JW3303**, seleccionar la longitud de onda de calibración en **$1550	ext{ nm}$** y fijar la atenuación inicial en **$0.00	ext{ dB}$** (modo absoluto o relativo según referencia).
2. Limpiar los conectores e intercalar el OVA en serie en el enlace troncal (por ejemplo, entre la salida del carrete **AB - 25 KM** y la entrada de línea del **Chasis DWDM 2** a través del ODF).
3. Verificar que el enlace DWDM permanezca operativo (`Link UP`) con la atenuación base mínima del dispositivo ($pprox 0.8	ext{ – }1.5	ext{ dB}$ de pérdida de inserción).

### B. Obtención y análisis del espectro con el OSA (VeEX RXT4510)
1. Conectar un *patch cord* limpio desde el puerto de monitoreo óptico (`MON`) del módulo MUX/DEMUX del chasis receptor hacia la entrada óptica del **RXT4510**.
2. Configurar el **RXT4510** en modo **DWDM**:
   - Seleccionar la grilla **ITU-T G.694.1** con espaciamiento de **100 GHz** (o **50 GHz** según configuración del MUX).
   - Ajustar el rango de barrido a **Banda C ($1528	ext{ nm} - 1568	ext{ nm}$)**.
3. Iniciar el barrido espectral y registrar los siguientes parámetros en la tabla de canales:
   - **Longitud de onda / Frecuencia medida** frente a la nominal ITU-T.
   - **Potencia de canal ($	ext{dBm}$)** (considerando el factor de acoplamiento del puerto `MON`, habitualmente $-20	ext{ dB}$, es decir, $1\%$ de potencia derivada).
   - **OSNR ($	ext{dB}$):** Relación señal a ruido óptica calculada entre el pico del canal y el piso de ruido ASE interpolado.
4. Incrementar gradualmente la atenuación en el **JW3303** en pasos de **$2	ext{ dB}$** y registrar la degradación de potencia y OSNR hasta alcanzar el umbral de corte del receptor.

### C. Medición de pérdida de tramas y BER con el Analizador Ethernet (VeEX MTX150x)
1. Conectar el puerto óptico/eléctrico del **MTX150x** al puerto de acceso del **Switch CSS610** (Nodo 1).
2. Configurar un bucle físico (*hard loop* con *patch cord* cruzado TX $
ightarrow$ RX) en el puerto de cliente equivalente del **Chasis DWDM 2** (o en un segundo puerto del nodo remoto) para cerrar el circuito bidireccional.
3. En el **MTX150x**, seleccionar la aplicación de prueba **BERT** (*Bit Error Rate Test*) o **RFC 2544**:
   - Configurar interfaz (1G o 10G), encapsulación Ethernet, tamaño de trama (ej. 64, 512, 1518 bytes o patrón IMIX) y patrón pseudoaleatorio **PRBS $2^{31}-1$**.
   - Iniciar la transmisión al $100\%$ de carga de línea durante un intervalo definido (ej. 60 segundos por punto de medición).
4. Registrar **Tasa de error de bit (BER)**, **tramas perdidas (*Frame Loss*)** y **latencia promedio** para cada nivel de atenuación aplicado en el OVA JW3303, identificando el punto exacto donde el FEC deja de corregir errores y aparece pérdida de paquetes.
