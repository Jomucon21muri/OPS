# TEMA 2: ELEMENTOS FUNCIONALES DE UN ORDENADOR DIGITAL. ARQUITECTURA

.............................................................................................

## ÍNDICE

2.1    INTRODUCCIÓN ............................................................................................35
       2.1.1  Contextualización ............................................................................37
2.2    UNIDAD CENTRAL DE PROCESO ............................................................................38
       2.2.1  Registros ...................................................................................38
       2.2.2  ALU .........................................................................................39
       2.2.3  La Unidad de Control .......................................................................40
2.3    LOS BUSES ...............................................................................................41
2.4    MEMORIA .................................................................................................42
       2.4.1  Memoria primaria ...........................................................................43
       2.4.2  Memorias de semiconductores: ROM y RAM ..................................................44
       2.4.3  Memoria Caché ..............................................................................44
       2.4.4  Memoria Virtual ............................................................................45
       2.4.5  Jerarquía de Memoria .......................................................................45
2.5    EL SISTEMA DE ENTRADA/SALIDA .........................................................................46
2.6    CICLO DE INSTRUCCIÓN ...................................................................................47
2.7    CONCLUSIÓN .............................................................................................49
2.8    BIBLIOGRAFÍA ...........................................................................................49

.............................................................................................

## 2.1. INTRODUCCIÓN

### 2.1.1. Contextualización

Un ordenador digital es un sistema que procesa información mediante la ejecución secuencial de instrucciones almacenadas en memoria. Su estructura fundamental fue establecida por John von Neumann en la máquina IAS (Institute for Advanced Study) en 1952, principio que perdura en la arquitectura de prácticamente todos los ordenadores modernos.

Los elementos funcionales de un ordenador digital son los componentes esenciales que permiten: (a) almacenar instrucciones y datos (memoria), (b) ejecutar operaciones sobre los datos (UCP), (c) comunicar información dentro del sistema (buses), (d) interactuar con dispositivos externos (E/S). La integración armónica de estos componentes define el rendimiento global del sistema.

Este tema analiza cada elemento funcional, su estructura interna, características de rendimiento y las relaciones entre componentes.

---

## 2.2. UNIDAD CENTRAL DE PROCESO

La UCP es el "cerebro" del ordenador, responsable de ejecutar instrucciones y coordinar todas las operaciones.

### 2.2.1. Registros

Los registros son pequeñas áreas de memoria ultrarrápida (< 1 ns) integradas en la UCP, usadas para almacenar datos temporales durante la ejecución.

**Tipos de registros:**

- *Registros de propósito general (GPR)*: almacenan operandos y resultados. Ejemplo (x86-32): EAX, EBX, ECX, EDX, ESI, EDI, EBP, ESP. Cantidad: típicamente 8-32 registros en CPU modernas.

- *Registros especiales*:
  - **Program Counter (PC) / Instruction Pointer (IP)**: contiene dirección de la próxima instrucción a ejecutar.
  - **Stack Pointer (SP)**: apunta al tope de la pila de datos.
  - **Status Register (SR) / Flags**: almacena bits de estado (carry, zero, sign, overflow, interrupt enable).

**Características:**
- Latencia: 1 ciclo de reloj.
- Throughput: acceso simultáneo a múltiples registros en ejecución paralela.
- En CPU de 64 bits: registros de 64 bits (RAX, RBX, ... en x86-64).

### 2.2.2. ALU (Unidad Aritmético-Lógica)

Circuito combinacional que ejecuta operaciones aritméticas y lógicas.

**Operaciones:**
- *Aritméticas*: suma, resta, multiplicación, división (moderna: en 1 ciclo suma/resta, 3-5 ciclos multiplicación).
- *Lógicas*: AND, OR, XOR, NOT, desplazamientos (shift left/right), rotaciones.
- *Comparación*: genera flags de resultado para instrucciones condicionales.

**Características:**
- Ancho: 8, 16, 32 o 64 bits según CPU.
- En CPU modernas: múltiples ALUs (4-6) para ejecución superescalar.
- Genera flags: Carry (C), Zero (Z), Sign (S), Overflow (V).

### 2.2.3. La Unidad de Control

Orquesta la ejecución de instrucciones generando señales de control que activan componentes apropiados.

**Funciones:**
- *Decodificación*: interpreta el código de operación (opcode) de la instrucción.
- *Microoperaciones*: algunos CPUs moderno descomponen instrucciones complejas en microoperaciones (µops). Ejemplo: una instrucción load-execute en x86 puede generar 3-4 µops.
- *Secuenciamiento*: genera secuencia de señales de control en tiempo preciso.
- *Sincronización*: coordina timing de lectura/escritura en registros, memoria, ALU.

**Implementación:**
- *Microcódigo* (histórico): ROM con secuencias de control predefinidas.
- *Hardware hard-wired* (moderno): lógica combinacional, más rápido.

---

## 2.3. LOS BUSES

Los buses son vías de comunicación que conectan componentes del ordenador, transmitiendo datos, direcciones y señales de control.

**Características:**
- *Ancho de datos*: 8, 16, 32, 64 bits (define cantidad de datos transferibles por ciclo).
- *Velocidad*: 100 MHz - 5+ GHz según tecnología.
- *Arbitraje*: protocolo para resolver conflictos cuando múltiples dispositivos solicitan bus.

**Tipos principales:**

- *FSB (Front Side Bus)*: conecta CPU a chipset/memoria. Velocidad típica: 400-1600 MHz. Ancho: 64 bits.
- *QPI (Intel Quick Path Interconnect)*: conexión entre CPUs multinúcleo. Ancho de banda: 25-51 GB/s.
- *PCIe (PCI Express)*: estándar para periféricos (discos, GPUs, NICs). PCIe 4.0: 4 GB/s por dirección (×16 = 64 GB/s teórico).
- *Buses internos (L1/L2 caché - UCP)*: latencia < 1 ns, ancho de banda > 100 GB/s.

---

## 2.4. MEMORIA

Sistema jerárquico de almacenamiento con trade-off velocidad-capacidad-costo.

### 2.4.1. Memoria Primaria

Almacenamiento rápido directamente accesible por UCP, requerido para ejecución de instrucciones.

**Requisitos:**
- Latencia baja: permita ciclos de UCP de 1-2 ns sin esperas.
- Capacidad: típicamente 4 GB - 64 GB en sistemas contemporáneos.
- Volatilidad: pierde contenido sin alimentación eléctrica.

### 2.4.2. Memorias de Semiconductores: ROM y RAM

**RAM (Random Access Memory):**
- *SRAM (Static)*: celdas de flip-flop, acceso muy rápido (< 10 ns), alto costo. Usado en cachés L1-L3.
- *DRAM (Dynamic)*: capacitor + transistor, latencia media (40-100 ns), bajo costo, requiere refresco. Usado en memoria principal.
  - Evolución: SDR (100-133 MHz, obsoleto) → DDR4 (2400-3200 MHz) → DDR5 (4800-6400 MHz).
  - Operaciones: RAS (Row Address Strobe), CAS (Column Address Strobe).

**ROM (Read-Only Memory):**
- PROM (Programmable): grabable una sola vez.
- EPROM (Erasable): borrable con luz UV.
- EEPROM: borrable eléctricamente, reprogramable.
- Uso: firmware, BIOS, bootloader.

### 2.4.3. Memoria Caché

Memoria rápida (SRAM) que almacena copias de datos accedidos recientemente desde memoria principal, reduciendo latencia promedio.

**Principios:**
- *Localidad temporal*: si se accede a un dato, es probable que se vuelva a acceder pronto.
- *Localidad espacial*: si se accede a dirección X, probablemente se accederá direcciones cercanas (X+1, X+2, ...).

**Niveles:**
- L1: 32 KB (instrucciones) + 32 KB (datos), latencia ~3-4 ciclos.
- L2: 256 KB - 1 MB, latencia ~10-20 ciclos.
- L3: 4-32 MB, latencia ~40-100 ciclos, compartida entre cores.

**Políticas:**
- *Reemplazo*: LRU (Least Recently Used), FIFO, random.
- *Escritura*: write-through (escribe simultáneamente en caché y memoria), write-back (diferido).

### 2.4.4. Memoria Virtual

Abstracción que permite que cada proceso vea espacio de direcciones continuo de 2^32 (32-bit) o 2^64 (64-bit) bytes, aunque la memoria física sea más pequeña.

**Ventajas:**
- Protección: procesos aislados en espacios virtuales independientes.
- Flexibilidad: permite sobreasignar memoria (overcommit).
- Relocalización: código se carga en cualquier ubicación física.

**Implementación:** MMU (Memory Management Unit) traduce direcciones virtuales a físicas mediante tablas de páginas. Tamaño de página típico: 4 KB (también 2 MB, 1 GB disponibles).

### 2.4.5. Jerarquía de Memoria

```
Nivel               Tamaño      Latencia    Ancho banda     Costo
─────────────────────────────────────────────────────────────────
Registros           < 1 KB      < 1 ns      > 1 TB/s        $$$$
L1 Caché            64 KB       3-4 ns      > 100 GB/s      $$$
L2 Caché            256KB-1MB   10-20 ns    50-100 GB/s     $$
L3 Caché            4-32 MB     40-100 ns   20-50 GB/s      $
DRAM                4-64 GB     60-100 ns   10-30 GB/s      $
SSD                 128GB+      1-10 µs     0.5-7 GB/s      $
HDD                 1TB+        10 ms       0.1 GB/s        $
```

---

## 2.5. EL SISTEMA DE ENTRADA/SALIDA

Subsistema que permite intercambio de datos entre ordenador y dispositivos externos (discos, redes, periféricos).

**Modos de E/S:**

- *Programada*: CPU consulta periódicamente estado del dispositivo (polling). Ineficiente, ocupa ciclos de CPU.
- *Interrupciones*: dispositivo señala a CPU cuando tiene datos listos. CPU atiende mediante rutina de interrupción.
- *DMA (Direct Memory Access)*: dispositivo accede directamente a memoria sin intervención de CPU, más eficiente para transferencias grandes.

**Componentes:**
- *Controlador E/S*: circuito intermediario que traduce protocolos de CPU a protocolos de periféricos.
- *Buffer*: almacenamiento temporal para desincronizar velocidades de CPU vs periféricos.
- *Puertos estándar*: serie (RS-232, USB), paralelo (antiguo), red (Ethernet, WiFi).

---

## 2.6. CICLO DE INSTRUCCIÓN

Secuencia repetitiva de etapas que ejecuta cada instrucción en el ordenador. Modelo clásico de 5 etapas:

1. **Fetch (IF)**: lectura de instrucción desde memoria usando PC como dirección. PC se incrementa.
   ```
   IR ← M[PC]
   PC ← PC + 4  (asumiendo instrucciones de 4 bytes)
   ```

2. **Decode (ID)**: decodificación de la instrucción, lectura de registros operandos.
   ```
   opcode ← IR[31:26]
   src1 ← Registros[IR[25:20]]
   src2 ← Registros[IR[19:14]]
   ```

3. **Execute (EX)**: operación en ALU sobre operandos.
   ```
   resultado ← ALU(src1, src2, opcode)
   ```

4. **Memory (MEM)**: si es load/store, acceso a memoria; si no, pasa resultado.
   ```
   si opcode = LOAD: dato ← M[dirección]
   si opcode = STORE: M[dirección] ← dato
   ```

5. **Write-back (WB)**: escribir resultado en registro destino.
   ```
   Registros[dst] ← resultado
   ```

**Tiempo de ciclo:** típicamente 1-3 ns en CPU modernas (3-5 GHz). En ideal, una instrucción completa por ciclo (CPI = 1).

**Problemas (hazards):**
- *Data hazard*: dependencia entre instrucciones consecutivas.
- *Control hazard*: saltos rompen predicción de PC.
- *Structural hazard*: múltiples instrucciones compiten por mismo recurso.

**Soluciones:**
- *Forwarding*: pasar resultado directamente entre etapas.
- *Stalling*: insertar ciclos de espera.
- *Predicción de saltos*: adivinar target del salto antes de ejecutarlo.

---

## 2.7. CONCLUSIÓN

Los elementos funcionales de un ordenador digital —UCP, memoria, buses, E/S— forman un sistema integrado cuyo rendimiento global depende del balanceo entre estos componentes. La arquitectura von Neumann, a pesar de sus limitaciones teóricas (cuello de botella von Neumann), ha demostrado ser extraordinariamente flexible y escalable a través de optimizaciones microarquitectónicas (paralelismo, caching, predicción).

Tendencias futuras: (a) heterogeneidad (cores grandes + pequeños), (b) memoria en-enclave, (c) aceleradores especializados integrados (AI, criptografía), (d) arquitecturas descentralizadas (no von Neumann).

---

## 2.8. BIBLIOGRAFÍA

1. Von Neumann, J. (1945). *First Draft of a Report on the EDVAC*. Moore School of Electrical Engineering, University of Pennsylvania.

2. Hennessy, J. L., & Patterson, D. A. (2017). *Computer Architecture: A Quantitative Approach* (6ª ed.). Morgan Kaufmann.

3. Tanenbaum, A. S., & Bos, H. (2014). *Modern Operating Systems* (4ª ed.). Pearson.

4. Stallings, W. (2015). *Computer Organization and Architecture* (10ª ed.). Pearson.

5. Smith, J. E., & Sohi, G. S. (2005). "The Microarchitecture of Superscalar Processors". *Proceedings of the IEEE*, 83(12), 1609-1624.

