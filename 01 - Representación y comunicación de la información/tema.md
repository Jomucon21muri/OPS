# TEMA 1: REPRESENTACIÓN Y COMUNICACIÓN DE LA INFORMACIÓN

.............................................................................................

## ÍNDICE

1. INTRODUCCIÓN ................................................................................................
   1.1. Contextualización ....................................................................................
   1.2. REPRESENTACIÓN DE DATOS NUMÉRICOS .........................................................
       1.2.1. Sistemas de numeración posicionales .................................................
       1.2.2. Cambio de base ................................................................................
       1.2.3. Operaciones básicas .........................................................................
       1.2.4. Enteros .......................................................................................
       1.2.5. Reales ........................................................................................
   1.3. REPRESENTACIÓN DE DATOS ALFANUMÉRICOS .....................................................
       1.3.1. ASCII .........................................................................................
       1.3.2. EBCDIC .......................................................................................
       1.3.3. Unicode .......................................................................................
   1.4. REPRESENTACIÓN DE INFORMACIÓN MULTIMEDIA ..................................................
       1.4.1. Imágenes .....................................................................................
       1.4.2. Sonido ........................................................................................
       1.4.3. Vídeo .........................................................................................
   1.5. COMUNICACIÓN DE LA INFORMACIÓN ..............................................................
   1.6. CONCLUSIÓN ...........................................................................................
   1.7. BIBLIOGRAFÍA .........................................................................................

.............................................................................................

---

## 1. INTRODUCCIÓN

### 1.1. Contextualización

La representación y comunicación de la información constituyen procesos fundamentales en los sistemas de información contemporáneos. La representación es el proceso de codificar información mediante formatos sintácticos, mientras que la comunicación es la transmisión de esa información a través de canales con garantías de integridad, seguridad y disponibilidad.

Desde la perspectiva de la ciencia de la computación, estos procesos deben considerar simultáneamente: (a) la fidelidad semántica, es decir, la preservación del significado tras la codificación y transmisión; (b) la eficiencia computacional y de almacenamiento; (c) la accesibilidad y usabilidad para diversos públicos; (d) la seguridad criptográfica y la protección de privacidad; (e) la conformidad normativa y legislativa.

La teoría de la información de Shannon (1948) proporciona el fundamento matemático: en un canal discreto sin ruido, la cantidad mínima de bits necesarios para codificar un símbolo de probabilidad p es -log₂(p) bits. La entropía de una fuente de información H(X) = -∑ p(x) log₂ p(x) cuantifica la información media por símbolo.

---

## 2. REPRESENTACIÓN DE DATOS NUMÉRICOS

### 2.1. Sistemas de Numeración Posicionales

Un sistema de numeración posicional utiliza la base b y posiciones para representar valores. Un número en base b con dígitos d_n...d_1d_0 tiene valor:
N = d_n × b^n + ... + d_1 × b^1 + d_0 × b^0

En informática: base binaria (b=2), octal (b=8), hexadecimal (b=16).

### 2.2. Cambio de Base

Conversión de base 10 a base b: división sucesiva por b, recogiendo restos en orden inverso.
Conversión de base b a base 10: aplicar fórmula posicional directamente.

Ejemplo: 13₁₀ = 1101₂ (verificación: 1×8 + 1×4 + 0×2 + 1×1 = 13)

### 2.3. Operaciones Básicas

Suma, resta, multiplicación y división en sistemas posicionales siguen reglas análogas a base 10, considerando acarreos y préstamos en la base correspondiente.

### 2.4. Enteros

**Representación de enteros con signo:**
- Signo-magnitud: bit más significativo indica signo (problema: +0 y -0).
- Complemento a uno: invertir todos los bits (problema: todavía +0 y -0).
- Complemento a dos (estándar): invertir todos los bits y sumar 1. Ventaja: única representación del cero, aritmética más simple.

Ejemplo (8 bits): -5 = 11111011₂ (complemento a dos de 00000101)

Rango representable en complemento a dos con n bits: -2^(n-1) a 2^(n-1)-1

### 2.5. Reales

**Notación científica normalizada:** m × 2^e, donde 1 ≤ m < 2

**Estándar IEEE 754:**
- Precisión simple (32 bits): 1 bit signo + 8 bits exponente + 23 bits mantisa. Rango ~10^(-38) a 10^38
- Precisión doble (64 bits): 1 bit signo + 11 bits exponente + 52 bits mantisa. Rango ~10^(-308) a 10^308

Valores especiales: +∞, -∞, NaN (Not a Number)

Problema de precisión: 0.1 + 0.2 ≠ 0.3 exactamente en punto flotante (error de representación)

---

## 3. REPRESENTACIÓN DE DATOS ALFANUMÉRICOS

### 3.1. ASCII

American Standard Code for Information Interchange (1963):
- 128 caracteres: 0-127
- 7 bits de información
- Limitado a alfabeto latino básico, dígitos, símbolos de puntuación
- Problema: no soporta caracteres acentuados, alfabetos no-latinos

### 3.2. EBCDIC

Extended Binary Coded Decimal Interchange Code:
- 256 caracteres
- Utilizado históricamente en mainframes IBM
- Menos portabilidad que ASCII/Unicode

### 3.3. Unicode

Universal Character Set (1991 onwards):
- Soporta 1.114.112 caracteres potenciales (U+0000 a U+10FFFF)
- Codificaciones: UTF-8 (variable 1-4 bytes, retrocompatible ASCII), UTF-16 (2-4 bytes), UTF-32 (4 bytes fijos)
- UTF-8 estándar de facto en web
- Ejemplos: 'A' = U+0041, 'ñ' = U+00F1, '中' = U+4E2D

---

## 4. REPRESENTACIÓN DE INFORMACIÓN MULTIMEDIA

### 4.1. Imágenes

**Rasterización (bitmap):**
- Matriz de píxeles, cada píxel con valor de color
- Profundidad de color: 1 bit (B/N), 8 bits (256 colores), 24 bits RGB (16.7M colores), 32 bits RGBA (con transparencia)
- Resolución: píxeles por pulgada (DPI). Típicamente: 72 DPI (pantalla), 300 DPI (impresión)

**Formatos:**
- **JPEG**: compresión con pérdida (DCT), ~90% reducción. Ideal para fotografías
- **PNG**: compresión sin pérdida, soporta transparencia. Ideal para gráficos
- **SVG**: formato vectorial (escalable), basado en XML

**Compresión JPEG:**
1. Convertir RGB a YCbCr (separar luminancia de crominancia)
2. Subamuestra crominancia (ojo menos sensible)
3. Aplicar Transformada Coseno Discreta (DCT)
4. Cuantizar coeficientes (descartar componentes no perceptibles)
5. Codificar con Huffman o aritmético

### 4.2. Sonido

**Muestreo digital (PCM - Pulse Code Modulation):**
- Tasa de muestreo: ciclos por segundo (Hz). Teorema Nyquist: frecuencia máxima representable = mitad tasa muestreo
- Estándar audio CD: 44.1 kHz, 16 bits/muestra, estéreo = 176.4 kB/s = 635 MB/hora
- Rango de audición humana: 20 Hz - 20 kHz

**Compresión de audio:**
- **MP3 (MPEG-3)**: compresión con pérdida (~128 kbps = 1/11 del original). Usa filtros psicoacústicos (enmascaramiento)
- **AAC (Advanced Audio Coding)**: mejor compresión que MP3
- **FLAC (Free Lossless Audio Codec)**: sin pérdida

### 4.3. Vídeo

**Codificación:**
- Descomposición en fotogramas (frames) a tasa (fps: frames por segundo). Cine: 24 fps, vídeo: 30 fps (NTSC) o 25 fps (PAL)
- Compresión: intraframe (JPEG aplicado a cada fotograma) e interframe (diferencias entre fotogramas)

**Formatos:**
- **MPEG-2**: vídeo digital, DVD
- **H.264 (MPEG-4 Part 10)**: ampliamente adoptado, YouTube
- **H.265 (HEVC)**: 50% menos tamaño que H.264 a igual calidad, 4K
- **VP9, AV1**: códecs abiertos alternativos

---

## 5. COMUNICACIÓN DE LA INFORMACIÓN

**Modelo de comunicación:** Emisor → Codificador → Canal → Decodificador → Receptor

**Canales de comunicación:**
- *Físicos*: par trenzado (Ethernet), fibra óptica (bajo ruido, alta velocidad), ondas de radio
- *Lógicos*: protocolos que estructuran transmisión sobre canales físicos

**Protocolos de comunicación (Modelo OSI):**

1. *Física*: transmisión de bits (voltajes, luz), velocidad en baud o bps
2. *Enlace*: marcos (frames), control de flujo. Ej.: Ethernet, PPP
3. *Red*: encaminamiento (routing). Ej.: IP (IPv4, IPv6)
4. *Transporte*: confiabilidad, control de congestión. Ej.: TCP (conexión confiable), UDP (datagrama sin conexión)
5. *Sesión*: gestión de diálogos, sincronización
6. *Presentación*: codificación, cifrado. Ej.: SSL/TLS, compresión
7. *Aplicación*: servicios al usuario. Ej.: HTTP, SMTP, FTP, DNS

**Capacidad de canal (Shannon):**
C = B × log₂(1 + S/N)

donde B = ancho de banda, S = potencia señal, N = potencia ruido. Límite teórico máximo de información transmitible.

---

## 6. CONCLUSIÓN

La representación y comunicación de información constituyen procesos complejos, interdisciplinarios y centrales en cualquier sistema informático. Una selección adecuada de formatos, protocolos y mecanismos de control de calidad requiere balancear múltiples requisitos contrapuestos: eficiencia de almacenamiento vs fidelidad, facilidad de uso vs seguridad, velocidad de transmisión vs confiabilidad.

La investigación actual se orienta hacia: (a) compresión semántica que preserve significado reduciendo volumen; (b) formatos auto-descriptivos para interoperabilidad; (c) mecanismos de seguridad integrados en la representación misma.





## 7. BIBLIOGRAFÍA

1. Shannon, C. E. (1948). "A Mathematical Theory of Communication". *Bell System Technical Journal*, 27, 379-423.

2. Hennessy, J. L., & Patterson, D. A. (2017). *Computer Architecture: A Quantitative Approach* (6ª ed.). Elsevier.

3. Tanenbaum, A. S. (2010). *Computer Networks* (5ª ed.). Pearson.

4. Stallings, W. (2014). *Cryptography and Network Security* (6ª ed.). Pearson.

5. Batini, C., & Scannapieco, M. (2016). *Data Quality: Concepts, Methodologies and Techniques*. Springer.

6. W3C (2021). *Web Content Accessibility Guidelines (WCAG) 2.1*. https://www.w3.org/WAI/WCAG21/quickref/

7. ISO/IEC (2016). *ISO/IEC 27001:2013 - Information technology, Security techniques, Information security management systems*.

---

## 8. LEGISLACIÓN

- **RGPD (Reglamento General de Protección de Datos)**, Reg. (UE) 2016/679: normativa principal en protección de privacidad en transmisión y tratamiento de datos personales en la UE. Establece derechos de acceso, rectificación y supresión.

- **Directiva (UE) 2016/2102 sobre accesibilidad de sitios web y aplicaciones móviles de organismos del sector público**: requiere conformidad WCAG 2.1 nivel AA mínimo.

- **LSSI-CE (Ley de Servicios de la Sociedad de la Información)**, Ley 34/1988: regulación española de servicios electrónicos y comercio electrónico.

- **ISO/IEC 27001:2013**: estándar internacional de sistemas de gestión de seguridad de la información.