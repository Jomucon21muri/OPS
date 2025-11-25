# TEMA 4: MEMORIA INTERNA. TIPOS. DIRECCIONAMIENTO. CARACTERÍSTICAS Y FUNCIONES

.............................................................................................

## ÍNDICE

4.1  INTRODUCCIÓN ................................................................................................66
     4.1.1 Contextualización ...............................................................................68
4.2  MEMORIA INTERNA ................................................................................................68
     4.2.1 Registros ..........................................................................................68
     4.2.2 Memoria Caché ...................................................................................69
     4.2.3 Memoria Principal ..............................................................................70
4.3  TIPOS DE MEMORIA ..........................................................................................70
     4.3.1 Memorias volátiles ..............................................................................70
     4.3.2 Memorias no volátiles ..........................................................................71
4.4  CONEXIÓN CON LA CPU. MECANISMOS DE DIRECCIONAMIENTO ..................................71
     4.4.1 Conexión con la CPU ...........................................................................71
     4.4.2 Mecanismos de direccionamiento: físico vs virtual ............................72
4.5  CARACTERÍSTICAS DE LAS MEMORIAS .....................................................................73
4.6  JERARQUÍA DE MEMORIAS. FUNCIONES ...........................................................75
4.7  CONCLUSIÓN ...................................................................................................76
4.8  BIBLIOGRAFÍA .............................................................................................76

.............................................................................................

## 4.1. INTRODUCCIÓN

### 4.1.1. Contextualización

La memoria interna constituye el subsistema que almacena instrucciones, datos y estado de ejecución en un ordenador. Su diseño y gestión condicionan directamente el rendimiento, la escalabilidad y la seguridad del sistema. La memoria no es una única entidad, sino una jerarquía de dispositivos y estructuras (registros, cachés, DRAM, memorias no volátiles) organizadas para optimizar la latencia y el ancho de banda frente al coste y la capacidad.

En este tema se analizan en profundidad las tecnologías de memoria, los mecanismos de direccionamiento y traducción de direcciones, las políticas de gestión y coherencia, y las características que determinan su comportamiento en aplicaciones reales.

---

## 4.2. MEMORIA INTERNA

### 4.2.1. Registros

Los registros son el nivel más próximo a la unidad de ejecución (ALU/FPU). Son memorias de acceso extremadamente rápido integradas en el propio núcleo de la CPU.

- Propiedades: acceso en 1 ciclo (o fracciones de ciclo), ancho normalmente igual al tamaño de la arquitectura (32, 64 bits), pequeño número de entradas (decenas).
- Tipos: registros arquitectónicos (visibles al programador) y registros físicos adicionales usados por mecanismos de renombrado para eliminar dependencias falsas.
- Función: mantener operandos temporales, contadores de programa (PC/IP), punteros de pila (SP/FP) y registros de estado/flags.

Impacto en rendimiento: un mayor número de registros visibles reduce operaciones de spill/restore a memoria, mejorando la densidad de instrucciones ejecutables sin acceder a la jerarquía lenta.

### 4.2.2. Memoria Caché

La caché es una memoria intermedia (SRAM) que guarda subconjuntos de la memoria principal para reducir la latencia de acceso medio.

- Organización: líneas de caché (cache lines), asociatividad (direct-mapped, set-associative, fully-associative), políticas de reemplazo (LRU, pseudo-LRU, CLOCK) y write policy (write-through, write-back).
- Estructura multinivel: L1 (split I/D), L2, L3 (por core o compartida). Los parámetros críticos son tamaño, latencia, ancho de banda y associatividad.

Modelo de acceso medio: la latencia media de acceso a nivel de programa puede modelarse como

  t_avg = H_L1 * t_L1 + (1 - H_L1) * (H_L2 * t_L2 + (1 - H_L2) * t_mem)

donde H_L1, H_L2 son las tasas de acierto en L1 y L2; t_L1, t_L2, t_mem son latencias respectivas.

Optimización: tamaños de línea balanceados entre locality spatial y coste de miss; prefetching (hardware/software) para explotar patrones de acceso; coherencia para sistemas multinúcleo.

### 4.2.3. Memoria Principal

La memoria principal (DRAM) es la memoria volátil de gran capacidad que almacena el espacio de direcciones físicamente accesible.

- Tecnología: DRAM con variaciones (SDRAM, DDRx) gestionada por controladores de memoria con parámetros temporales (tCAS, tRCD, tRP, tREFI).
- Características: latencias mayores que SRAM, mayor densidad y menor coste por bit.
- Interfaz: canales de memoria, ranks y banks; interleaving para aumentar paralelismo.

El diseño del controlador de memoria y políticas como row-buffer management (open/close page) influyen notablemente en la latencia observada y el ancho de banda efectivo.

---

## 4.3. TIPOS DE MEMORIA

### 4.3.1. Memorias volátiles

- **SRAM (Static RAM)**: celdas basadas en flip-flops; muy rápidas, usadas en caches; consumo estático notable; coste por bit alto.
- **DRAM (Dynamic RAM)**: celdas capacitor+transistor; requieren refresco periódico; densidad alta; latencia y coste intermedios.
- **SRAM as embedded**: en SoC, bloques de SRAM integrados para buffers y caches locales.

### 4.3.2. Memorias no volátiles

- **ROM / PROM / EEPROM / Flash NAND**: almacenan firmware y datos persistentes; Flash NAND domina en SSDs; presentan latencias de escritura mucho mayores y desgaste por ciclos.
- **Memorias persistentes emergentes (NVDIMM, Intel Optane PMEM)**: ofrecen latencias intermedias entre DRAM y SSD y persistencia; abren nuevos modelos de programación (byte-addressable persistence).

Implicaciones prácticas: la elección de memoria (DRAM vs NVM) implica compensaciones en latencia, ancho de banda, coste y durabilidad; los sistemas modernos combinan niveles para mejores prestaciones.

---

## 4.4. CONEXIÓN CON LA CPU. MECANISMOS DE DIRECCIONAMIENTO

### 4.4.1. Conexión con la CPU

La CPU accede a memoria mediante controladores y buses (o enlaces punto a punto en arquitecturas modernas). Elementos relevantes:

- **Controlador de memoria**: gestiona transacciones, refresh de DRAM, reordenamiento de requests (memory scheduler) para maximizar throughput y reducir latencia media.
- **Interconexión**: canales DDR, enlaces en SoC (AXI, AHB), o enlaces coherentes (QPI, Infinity Fabric) en sistemas multiprocesador.
- **Buffers y filas**: read/write queues, write combining, y reordenamiento afectan latencia y fairness.

### 4.4.2. Mecanismos de direccionamiento: físico vs virtual

- **Direccionamiento físico**: la dirección corresponde a una ubicación en la memoria física. El acceso es directo pero no proporciona aislamiento entre procesos.

- **Direccionamiento virtual**: cada proceso ve su propio espacio de direcciones virtuales que la MMU (Memory Management Unit) traduce a direcciones físicas mediante tablas de páginas. Beneficios: protección, compartición controlada, memory overcommit.

Traducción de direcciones: tablas de páginas multinivel (p. ej., PML4/PDPT/PDE/PTE en x86-64) permiten gestionar espacios de 48-64 bits sin tablas monolíticas. Las entradas contienen bits de permisos (R/W/X), atributo de caché (PAT), y flags de present/dirty/accessed.

**TLB (Translation Lookaside Buffer)**: cache de traducciones VA→PA. Un TLB miss implica walking de tablas en memoria (page table walk) y costes de cientos de ciclos si no hay hardware de walking eficiente.

**Tamaños de página**: 4 KB (típico), 2 MB, 1 GB (hugepages) para reducir overhead de TLB y page-table walking; trade-off: internal fragmentation.

**Modelos de direccionamiento y endianess**: la forma en que bytes se ordenan (big/little endian) influye en interpretación binaria y compatibilidad entre arquitecturas.

---

## 4.5. CARACTERÍSTICAS DE LAS MEMORIAS

Analizaremos las propiedades que definen el comportamiento de cada nivel:

1. **Latencia**: tiempo desde la petición hasta la recepción de datos (ns, µs, ms). Registros y L1 medidos en ns; DRAM decenas de ns; SSD µs; HDD ms.
2. **Ancho de banda**: bytes/s que puede transportar el canal de memoria; influye en throughput.
3. **Densidad**: bits por mm^2; condiciona capacidad y coste.
4. **Volatilidad**: pérdida de datos sin alimentación.
5. **Endurance**: número de ciclos de escritura soportados (crítico en flash).
6. **Consumo energético**: energía por acceso y en reposo (importante en sistemas móviles y centros de datos).
7. **Consistencia y modelo de memoria**: define la visibilidad de escrituras entre hilos/cores (sequential consistency, weak consistency). La implementación hardware y el SO proporcionan barreras (fence), atomic operations y semánticas de ordenamiento.

Fórmulas y métricas

- Tiempo medio de acceso (ejemplo multilevel cache): ya presentado en 4.2.2.
- Bandwidth-latency product: una medida de capacidad efectiva para streams de datos.

Consideraciones de diseño: optimizaciones para reducir latencia efectiva incluyen prefetchers, non-blocking caches, hardware prefetch y paralelismo a nivel de bancos.

---

## 4.6. JERARQUÍA DE MEMORIAS. FUNCIONES

La jerarquía busca minimizar el coste medio por acceso manteniendo rendimiento:

- **Registros**: operandos inmediatos.
- **Cachés (L1–L3)**: reducir misses a memoria principal.
- **Memoria principal (DRAM)**: espacio de trabajo de procesos.
- **Memoria secundaria (SSD/HDD)**: persistencia masiva.

Funciones específicas:
- *Reducción de latencia*: caches absorben accesos frecuentes.
- *Aislamiento y seguridad*: MMU y protecciones de página impiden accesos no autorizados.
- *Eficiencia energética*: mover datos entre niveles según coste energético.

Coherencia en entornos multinúcleo: protocolos MESI/MOESI evitan inconsistencias entre caches; estructuras de directorio en sistemas a gran escala escalan mejor que la snooping en topologías con muchos cores.

---

## 4.7. CONCLUSIÓN

La memoria interna es un subsistema multisistema que condiciona arquitectura y programación. Su comprensión exige trabajar simultáneamente sobre tecnología (SRAM/DRAM/NVM), estructuras de datos y algoritmos (p. ej., layout para locality), y políticas de gestión (p. ej., replacement, page allocation). El futuro inmediato muestra la convergencia de memoria persistente y volátil, heterogeneidad en jerarquías y nuevas APIs para persistencia directa.

Profesionalmente, dominar la memoria interna permite optimizar rendimiento (latencia y throughput), reducir consumo y diseñar sistemas seguros y escalables.

---

## 4.8. BIBLIOGRAFÍA

1. Hennessy, J. L., & Patterson, D. A. (2017). *Computer Architecture: A Quantitative Approach* (6ª ed.). Morgan Kaufmann.

2. Jacob, B., Ng, S., & Wang, D. (2007). *Memory Systems: Cache, DRAM, Disk*. Morgan Kaufmann.

3. Tanenbaum, A. S., & Bos, H. (2014). *Modern Operating Systems* (4ª ed.). Pearson.

4. Patterson, D. A., & Hennessy, J. L. (2013). *Computer Organization and Design* (5ª ed.). Morgan Kaufmann.

5. JEDEC Solid State Technology Association. *DDRx SDRAM Standards* (JESD79 series).

---

## LEGISLACIÓN RELEVANTE

- **ISO/IEC 27001:2013**: sistemas de gestión de seguridad de la información; requisitos aplicables al tratamiento y protección de datos en memoria.
- **RGPD (Reglamento General de Protección de Datos)**, Reg. (UE) 2016/679: obligaciones en integridad, confidencialidad y disponibilidad de datos personales almacenados en sistemas.
- **JEDEC standards (JESD79 etc.)**: estándares de la industria sobre DRAM/SDRAM que regulan interoperabilidad y parámetros eléctricos/timings.
