<div align="center">

# VM-01

### Prototipo de ventilador mecánico electrónico

*Control por microcontrolador · Electroválvulas neumáticas · Monitoreo de presión*

<br>

![Progreso](https://img.shields.io/badge/Progreso-0%25-lightgrey?style=for-the-badge)
![Componentes](https://img.shields.io/badge/Componentes-0%20%2F%2066-blue?style=for-the-badge)
![Entrega](https://img.shields.io/badge/Entrega-Noviembre%202026-orange?style=for-the-badge)

<br>

`Arduino Mega 2560` · `Airtac 2V130 / 2V025` · `HX710B` · `LCD 20x4 I2C`

<br>

**Juan David Cabarcas Contreras**
Ingeniería Biomédica · Corporación Universitaria Reformada

</div>

<br>

---

<br>

## Índice

<table>
<tr>
<td width="50%" valign="top">

**Materiales**

`01` &nbsp; [Control y electrónica](#01--control-y-electrónica)
`02` &nbsp; [Etapa de potencia](#02--etapa-de-potencia)
`03` &nbsp; [Alimentación](#03--alimentación)
`04` &nbsp; [Conectores y cableado](#04--conectores-y-cableado)
`05` &nbsp; [Interfaz de panel](#05--interfaz-de-panel)

</td>
<td width="50%" valign="top">

**Construcción**

`06` &nbsp; [Neumática del equipo](#06--neumática-del-equipo)
`07` &nbsp; [Circuito del paciente](#07--circuito-del-paciente)
`08` &nbsp; [Gabinete y montaje](#08--gabinete-y-montaje)
`09` &nbsp; [Fabricación de la PCB](#09--fabricación-de-la-pcb)

</td>
</tr>
<tr>
<td colspan="2">

**Gestión** — [Cronograma](#cronograma) · [Limitaciones del diseño](#limitaciones-del-diseño) · [Seguridad](#notas-de-seguridad) · [Instrucciones](#cómo-usar-este-documento)

</td>
</tr>
</table>

<br>

---

<br>

## Arquitectura del sistema

```
                         ┌──────────────────┐
   Cilindro              │   ARDUINO MEGA   │
   de aire               │     2560         │
      │                  └────────┬─────────┘
      │                           │
      ▼                  ┌────────┴─────────┐
 ┌─────────┐             │                  │
 │Regulador│        ┌────▼────┐      ┌──────▼──────┐
 │ 2-3 bar │        │ LCD 20x4│      │  2x IRLZ44N │
 └────┬────┘        │ Encoder │      └──────┬──────┘
      │             │ Pilotos │             │
      ▼             │ Buzzer  │      ┌──────┴──────┐
 ┌─────────┐        └─────────┘      │             │
 │ Filtro  │                    ┌────▼───┐   ┌─────▼──┐
 └────┬────┘                    │ VÁLV.  │   │ VÁLV.  │
      │                         │ INSP.  │   │ ESP.   │
      ▼                         │2V130-10│   │2V025-08│
 ┌─────────┐                    └────┬───┘   └────▲───┘
 │  Aguja  ├─────────────────────────┘            │
 └─────────┘                         │            │
                                     ▼            │
                              ┌──────────┐        │
                     ┌────────┤    T     │        │
                     │        └─────┬────┘        │
                ┌────▼────┐         │             │
                │ SENSOR  │         ▼             │
                │ HX710B  │   ╔═══════════╗       │
                └─────────┘   ║  PACIENTE ║───────┘
                              ║  + PEEP   ║
                              ╚═══════════╝
```

<br>

---

<br>

## Cómo usar este documento

<table>
<tr>
<td width="33%" valign="top">

### Marcado

Marca la casilla cuando tengas el componente **físicamente en mano**, no cuando lo pidas.

Edita el archivo y cambia `[ ]` por `[x]`.

</td>
<td width="33%" valign="top">

### Prioridades

🔴 &nbsp;**Crítico**
Sin esto no avanza el proyecto.

🟡 &nbsp;**Importante**
Se puede improvisar temporalmente.

🟢 &nbsp;**Acabado**
Mejora la presentación.

</td>
<td width="33%" valign="top">

### Estado de pedidos

⏳ &nbsp;Pedido, en camino
✅ &nbsp;Recibido y verificado
❌ &nbsp;Pendiente de compra

Anótalo junto al ítem si necesitas seguimiento.

</td>
</tr>
</table>

<br>

### Avance por sección

| Sección | Ítems | Hechos | Barra |
|:--|:-:|:-:|:--|
| `01` Control y electrónica | 7 | 0 | `░░░░░░░░░░` 0% |
| `02` Etapa de potencia | 9 | 0 | `░░░░░░░░░░` 0% |
| `03` Alimentación | 6 | 0 | `░░░░░░░░░░` 0% |
| `04` Conectores y cableado | 6 | 0 | `░░░░░░░░░░` 0% |
| `05` Interfaz de panel | 2 | 0 | `░░░░░░░░░░` 0% |
| `06` Neumática del equipo | 11 | 0 | `░░░░░░░░░░` 0% |
| `07` Circuito del paciente | 7 | 0 | `░░░░░░░░░░` 0% |
| `08` Gabinete y montaje | 9 | 0 | `░░░░░░░░░░` 0% |
| `09` Fabricación de la PCB | 10 | 0 | `░░░░░░░░░░` 0% |
| **TOTAL** | **67** | **0** | `░░░░░░░░░░` **0%** |

<br>

---

<br>

## `01` · Control y electrónica

> Cerebro del equipo, interfaz de usuario y captura de datos.

| ✔ | | Componente | Cant. | Función |
|:-:|:-:|:--|:-:|:--|
| [ ] | 🔴 | **Arduino Mega 2560** | 1 | Ejecuta la lógica de ciclos, lee el sensor y comanda las válvulas |
| [ ] | 🔴 | **LCD 20x4 con módulo I2C** | 1 | Muestra frecuencia, volumen, presión, I:E y alarmas |
| [ ] | 🔴 | **Encoder rotatorio KY-040** | 1 | Ajusta parámetros y navega el menú con un solo control |
| [ ] | 🟡 | **Buzzer pasivo 5V** | 1 | Alarma audible con tonos según prioridad |
| [ ] | 🔴 | **Sensor de presión HX710B** `0–40 kPa` | 3 | Mide presión de vía aérea; habilita alarmas y modo PC |
| [ ] | 🟡 | **Protoboard 830 puntos** | 1 | Validar el circuito antes de fabricar la PCB |
| [ ] | 🟡 | **Kit cables Dupont** M-M y M-H | 1 | Interconexión durante pruebas |

<details>
<summary><b>Notas de esta sección</b></summary>

<br>

**Prioridad de compra.** El sensor HX710B tarda de 2 a 3 semanas en llegar. Debe ser el primer pedido que salga.

**Verificar al recibir la LCD.** Confirmar que el módulo I2C (chip PCF8574) viene ya soldado en la parte trasera. Algunas publicaciones venden la pantalla sola.

**Asignación de pines prevista.**

| Pin | Componente | Pin | Componente |
|:-:|:--|:-:|:--|
| `20` | SDA — LCD | `6` | Buzzer |
| `21` | SCL — LCD | `22` | LED verde |
| `8` | Válvula inspiratoria | `24` | LED amarillo |
| `9` | Válvula espiratoria | `26` | LED azul |
| `2` | Encoder CLK | `28` | LED rojo |
| `3` | Encoder DT | `30` | Sensor OUT |
| `4` | Encoder SW | `31` | Sensor SCK |
| `5` | Botón de silencio | | |

Los pines `2` y `3` son de interrupción por hardware en el Mega — el encoder no pierde pasos aunque el programa esté ocupado.

</details>

<br>

---

<br>

## `02` · Etapa de potencia

> Traduce las señales de 5V del Arduino a los 12V que necesitan las cargas.

| ✔ | | Componente | Cant. | Función |
|:-:|:-:|:--|:-:|:--|
| [ ] | 🔴 | **MOSFET IRLZ44N** | 3 | Conmuta las electroválvulas. Nivel lógico, opera con 5V de compuerta |
| [ ] | 🟡 | **Transistor 2N2222** | 6 | Conmuta los pilotos LED de 12V desde pines de 5V |
| [ ] | 🔴 | **Diodo 1N4007** | 10 | Flyback sobre cada bobina + protección de polaridad en VIN |
| [ ] | 🔴 | **Resistencia 220Ω** | 10 | En serie con la compuerta de cada MOSFET |
| [ ] | 🟡 | **Resistencia 1kΩ** | 10 | Base de los transistores de los pilotos |
| [ ] | 🔴 | **Resistencia 10kΩ** | 10 | Pull-down de compuerta; evita activación al arrancar |
| [ ] | 🟡 | **Condensador 1000µF / 25V** | 1 | Absorbe picos de corriente al conmutar |
| [ ] | 🟡 | **Condensador 100µF / 25V** | 1 | Estabiliza la entrada de alimentación del Arduino |
| [ ] | 🟢 | **Disipador TO-220** | 2 | Refrigera los MOSFET en operación continua |

<details>
<summary><b>⚠ Advertencia — elección del MOSFET</b></summary>

<br>

La referencia **debe llevar la letra L** (de *logic level*): `IRLZ44N`, `IRL540N`, `IRLB8721`, `FQP30N06L`.

Los modelos sin L — `IRF520`, `IRF540`, `IRFZ44` — requieren **10V** en la compuerta. Con los 5V del Arduino conducen a medias, se calientan y fallan de forma intermitente. Es el error más común en este tipo de montaje.

**Cómo verificar en la tienda:** en la hoja de datos, el parámetro `V_GS(th)` debe ser menor a 2V, y las curvas deben especificar corriente a `V_GS = 4.5V` o `5V`.

</details>

<details>
<summary><b>Circuito por cada válvula</b></summary>

<br>

```
  +12V ─────┬──────────────┐
            │              │
          ┌─┴─┐          ──┴──  D  (1N4007)
          │   │           ─┬─      banda hacia +12V
          │VÁL│            │
          │VUL│            │
          │ A │            │
          └─┬─┘            │
            └──────────────┤
                           │
                     ┌─────┴─────┐
    Pin Arduino      │  D        │
        │            │           │
       220Ω ─────────┤ G IRLZ44N │
        │            │           │
       10kΩ          │  S        │
        │            └─────┬─────┘
       GND ───────────────-┴────── GND
```

El diodo flyback no es opcional: sin él, el pico de voltaje al cerrar la bobina destruye el MOSFET.

</details>

<br>

---

<br>

## `03` · Alimentación

> Los 110V permanecen **fuera** del gabinete. Solo entran 12V.

| ✔ | | Componente | Cant. | Función |
|:-:|:-:|:--|:-:|:--|
| [ ] | 🔴 | **Fuente conmutada 12V / 3A** `36W` | 1 | Alimenta válvulas, pilotos y Arduino. Queda externa |
| [ ] | 🔴 | **Jack DC hembra de panel** `5.5×2.1 mm` | 1 | Entrada de 12V en la cara trasera, con tuerca |
| [ ] | 🟡 | **Cable plug DC macho** `5.5×2.1` | 1 | Lleva 12V de la placa al jack del Arduino |
| [ ] | 🟡 | **Interruptor basculante iluminado 12V** | 1 | Encendido, cortando la línea de 12V |
| [ ] | 🔴 | **Portafusible de panel + fusible 2A** | 1 | Protege ante cortocircuito |
| [ ] | 🔴 | **Cable 20 AWG** rojo y negro | 2 m | Distribución interna de 12V |

<details>
<summary><b>Cálculo de carga</b></summary>

<br>

| Carga | Consumo |
|:--|--:|
| Válvula inspiratoria `7W / 12V` | 0.58 A |
| Válvula espiratoria `7W / 12V` | 0.58 A |
| Arduino + LCD + sensor + encoder | ~0.30 A |
| Pilotos LED `4 × 20mA` | 0.08 A |
| **Total** | **≈ 1.54 A** |
| **Capacidad de la fuente** | **3.00 A** |
| **Margen** | **≈ 49%** |

Una fuente de 1A **no alcanza**. El amperaje de más nunca es problema: el circuito toma solo lo que consume. El voltaje sí debe coincidir exactamente.

</details>

<br>

---

<br>

## `04` · Conectores y cableado

> Todo lo que sale de la placa va con conector desmontable. Abrir el equipo no debe requerir desoldar.

| ✔ | | Componente | Cant. | Función |
|:-:|:-:|:--|:-:|:--|
| [ ] | 🔴 | **Bornera enchufable KF2EDG 2 vías** | 4 juegos | Potencia: entrada 12V, dos válvulas, interruptor |
| [ ] | 🔴 | **Kit JST XH con crimpadora** | 1 | Señal: LCD, encoder, sensor, pilotos, botón, buzzer |
| [ ] | 🟡 | **Header macho 2.54 mm** (tira) | 2 | Puntos de conexión hacia el Arduino |
| [ ] | 🔴 | **Cable multifilar 22 AWG** varios colores | 5 m | Latiguillos entre placa y panel |
| [ ] | 🟡 | **Kit termorretráctil** surtido | 1 | Aislar cada empalme |
| [ ] | 🟢 | **Amarres con base adhesiva** | 1 paq. | Organizar el cableado interno |

<details>
<summary><b>Mapa de conectores de la placa</b></summary>

<br>

| Conector | Tipo | Vías | Destino |
|:--|:--|:-:|:--|
| `12V IN` | KF2EDG | 2 | Jack de panel |
| `SW + FUSE` | KF2EDG | 2 | Interruptor y portafusible |
| `VALV-INSP` | KF2EDG | 2 | Electroválvula inspiratoria |
| `VALV-ESP` | KF2EDG | 2 | Electroválvula espiratoria |
| `LCD` | JST XH | 4 | VCC, GND, SDA, SCL |
| `ENCODER` | JST XH | 5 | +, GND, CLK, DT, SW |
| `SENSOR` | JST XH | 4 | VCC, GND, OUT, SCK |
| `LEDS` | JST XH | 5 | 4 señales + GND común |
| `MUTE` | JST XH | 2 | Botón de silencio |
| `BUZZ` | JST XH | 2 | Buzzer |

**Regla de seguridad.** KF2EDG solo para potencia, JST XH solo para señal. Al ser físicamente incompatibles, es imposible enchufar 12V donde va una señal de 5V.

**Serigrafía.** Rotular cada conector en la placa. Es lo que diferencia un montaje profesional de uno improvisado, y evita errores al rearmar semanas después.

</details>

<br>

---

<br>

## `05` · Interfaz de panel

| ✔ | | Componente | Cant. | Función |
|:-:|:-:|:--|:-:|:--|
| [ ] | 🟡 | **Piloto LED de panel** `Ø8 mm` (set 10) | 1 set | Indicadores: red, inspiración, espiración, alarma |
| [ ] | 🟡 | **Pulsador de panel** | 1 | Silencio de alarma — botón físico independiente |

<details>
<summary><b>Código de indicadores</b></summary>

<br>

| Color | Estado | Significado |
|:-:|:--|:--|
| 🟢 | `RED` | Equipo energizado, operación normal |
| 🟠 | `INSP` | Fase inspiratoria activa |
| 🔵 | `ESP` | Fase espiratoria activa |
| 🔴 | `ALARMA` | Presión fuera de rango o desconexión |

Los indicadores de fase permiten seguir el ciclo respiratorio sin mirar la pantalla — útil en la sustentación.

**Verificar el voltaje.** Si los pilotos son de 12V requieren los transistores 2N2222 de la sección `02`. La versión de 5V se conecta directo al Arduino y permite omitirlos.

</details>

<br>

---

<br>

## `06` · Neumática del equipo

> Todo lo que va dentro del gabinete, desde la entrada de aire hasta los puertos del paciente.

| ✔ | | Componente | Cant. | Función |
|:-:|:-:|:--|:-:|:--|
| [ ] | 🔴 | **Electroválvula Airtac 2V130-10** `12V DC` | 1 | **Inspiratoria.** Orificio 13 mm, puerto 3/8" |
| [ ] | 🔴 | **Electroválvula Airtac 2V025-08F** `12V DC` | 1 | **Espiratoria.** Orificio 2.5 mm, acción directa, abre desde 0 bar |
| [ ] | 🔴 | **Regulador de presión con manómetro** | 1 | Baja la presión del cilindro a 2–3 bar |
| [ ] | 🔴 | **Válvula de aguja 1/4"** | 1 | Limita el caudal inspiratorio; calibra el volumen |
| [ ] | 🟡 | **Filtro neumático miniatura** | 2 | Retiene partículas en entrada y escape |
| [ ] | 🔴 | **Racor PC 8-03 o codo PL 8-03** | 2 | Conexión a la válvula inspiratoria `rosca 3/8"` |
| [ ] | 🔴 | **Racor PC 6-02 o codo PL 6-02** | 2 | Conexión a la válvula espiratoria `rosca 1/4"` |
| [ ] | 🔴 | **Passamuro PM 6** | 4 | Entrada aire, escape, puerto insp., puerto esp. |
| [ ] | 🔴 | **T neumática PE 6** | 1 | Derivación hacia el sensor de presión |
| [ ] | 🔴 | **Manguera poliuretano 6 mm** | 2 m | Interconexión interna |
| [ ] | 🔴 | **Manguera silicona 4 mm** | 0.5 m | Derivación al sensor |

<details>
<summary><b>Por qué estas válvulas y no otras</b></summary>

<br>

| Serie | Orificio | Presión mínima | Apta para |
|:--|:-:|:-:|:--|
| `2V025` | 2.5 mm | **0 bar** — acción directa | Ambos lados |
| `2V130` | 13 mm | 0.5 bar — piloto interno | Solo inspiratorio |
| `2V250` | 25 mm | 0.5 bar — piloto interno | Solo inspiratorio |
| `4V210 / 4V220` | — | 1.5 bar, 5/2 vías | ❌ No aptas |

**Lado inspiratorio.** Dispone de 2–3 bar del regulador, así que la `2V130-10` opera sin problema y su orificio de 13 mm multiplica el caudal frente a los 2.5 mm.

**Lado espiratorio.** El aire sale del pulmón a ~0.03 bar. Solo una válvula de acción directa abre en ese rango, y la `2V025` es la mayor de esa familia. Se compensa con tiempos espiratorios largos.

**Las 4V210 y 4V220 no sirven** aunque sean más baratas: son válvulas 5/2 diseñadas para accionar cilindros neumáticos, no para dirigir flujo respiratorio, y exigen 1.5 bar mínimo.

</details>

<details>
<summary><b>⚠ Usar aire comprimido, no oxígeno</b></summary>

<br>

Las válvulas neumáticas industriales vienen lubricadas de fábrica. El oxígeno a presión en contacto con aceites o grasas representa **riesgo de ignición**.

Para uso con oxígeno se requieren válvulas con certificación específica, libres de hidrocarburos en cuerpo y sellos. Fuera del alcance de este prototipo.

</details>

<details>
<summary><b>Recorrido del aire</b></summary>

<br>

**Rama inspiratoria**

`Entrada trasera` → `Filtro` → `Válvula de aguja` → `Válvula inspiratoria` → `T de derivación` → `Puerto inspiratorio`

De la T sale una manguerita fina hacia el sensor de presión.

**Rama espiratoria**

`Puerto espiratorio` → `Válvula espiratoria` → `Escape trasero con filtro`

**Orden que no debe alterarse.** La válvula de aguja va **antes** de la electroválvula. Si se instala después, el golpe de presión llega al paciente antes de ser limitado.

</details>

<br>

---

<br>

## `07` · Circuito del paciente

> Va por fuera del equipo. **Gestionar en la clínica antes de comprar** — son insumos desechables.

| ✔ | | Componente | Cant. | Función |
|:-:|:-:|:--|:-:|:--|
| [ ] | 🔴 | **Manguera corrugada 22 mm** | 2 m | Ramas inspiratoria y espiratoria |
| [ ] | 🔴 | **Pieza en Y 22 mm** | 1 | Une las dos ramas junto al paciente |
| [ ] | 🔴 | **T de 22 mm** | 1 | Soporta la válvula de alivio |
| [ ] | 🔴 | **Válvula PEEP / pop-off** `~40 cmH₂O` | 1 | **Seguridad mecánica** independiente del software |
| [ ] | 🟡 | **Adaptadores 22 mm a espiga** | 4 | Acople al equipo |
| [ ] | 🔴 | **Pulmón de prueba o ambú** | 1 | Simula al paciente en las pruebas |
| [ ] | 🟢 | **Filtro bacteriano** | 1 | Criterio clínico; simbólico en el prototipo |

<details>
<summary><b>Alternativa a la válvula PEEP — columna de agua</b></summary>

<br>

Si no se consigue una válvula PEEP, el mismo principio se logra con un **sello de agua**: un tubo sumergido a la profundidad deseada dentro de un frasco.

```
      Desde la T
          │
          ▼
     ┌────┴────┐
     │         │
     │  ╷      │
     │  │      │  ← tubo sumergido 40 cm
     │ ~│~~~~~ │  ← nivel de agua
     │  │      │
     │  °   °  │  ← burbujea sobre 40 cmH₂O
     └─────────┘
```

Cuando la presión supera los 40 cmH₂O, el aire burbujea y se libera. Es el mismo principio de los drenajes torácicos.

**Ventajas para la sustentación:** cuesta casi nada, el burbujeo es visible y audible, y permite explicar por qué la seguridad crítica no puede depender del software.

</details>

<details>
<summary><b>Ubicación de la válvula de alivio</b></summary>

<br>

Va **fuera del equipo**, intercalada en la manguera inspiratoria mediante una T de 22 mm, con la salida **hacia arriba** y lo más cerca posible de la pieza en Y.

Si se instala acostada o hacia abajo, el peso del mecanismo altera la presión de apertura y acumula condensación.

</details>

<br>

---

<br>

## `08` · Gabinete y montaje

> Formato **vertical**: 120 mm ancho × 200 mm alto × 75 mm fondo.

| ✔ | | Componente | Cant. | Función |
|:-:|:-:|:--|:-:|:--|
| [ ] | 🔴 | **Caja plástica 200×120×75 mm** | 1 | Gabinete principal, montado en vertical |
| [ ] | 🟢 | **Vinilo adhesivo impreso** | 1 | Panel frontal. Se pega **por dentro**, impreso en espejo |
| [ ] | 🔴 | **Tornillos M3 × 12 mm** cabeza plana negra | 10 | Fijación de LCD y placas |
| [ ] | 🔴 | **Separadores hexagonales M3 de 6 mm** | 10 | Espacio entre tapa y LCD; evita forzar el vidrio |
| [ ] | 🔴 | **Tuercas y arandelas M3** | 20 | Complemento de tornillería |
| [ ] | 🟢 | **Patas de goma adhesivas** | 4 | Estabilidad en formato vertical |
| [ ] | 🟡 | **Pasacables de goma** | 6 | Protegen el aislamiento en cada perforación |
| [ ] | 🟡 | **Abrazaderas para válvulas** | 2 | Fijan las válvulas al fondo |
| [ ] | 🟡 | **Broca de copa y lima** | — | Perforaciones Ø5, Ø8, Ø12 mm |

<details>
<summary><b>Plano de cortes del panel frontal</b></summary>

<br>

Coordenadas desde la esquina superior izquierda de la tapa `120 × 200 mm`.

| Elemento | Centro `x, y` | Corte |
|:--|:-:|:--|
| Ventana LCD | `60, 74` | Rectángulo 98 × 38 mm |
| Encoder | `60, 137` | Ø 7 mm |
| Piloto verde | `22, 188` | Ø 8 mm |
| Piloto amarillo | `47, 188` | Ø 8 mm |
| Piloto azul | `72, 188` | Ø 8 mm |
| Piloto rojo | `97, 188` | Ø 8 mm |
| Botón de silencio | `26, 218` | Ø 12 mm |

**Verificar la ventana antes de cortar.** El área visible de una LCD 20x4 ronda los 76 × 26 mm, pero el módulo completo mide 98 × 60 mm. El corte debe dejar ver el vidrio sin exponer la placa.

**Margen ajustado.** La LCD mide 98 mm de ancho sobre una tapa de 120 mm: quedan 11 mm por lado. Confirmar que los agujeros de montaje no choquen con las esquinas redondeadas.

</details>

<details>
<summary><b>⚠ Orden de trabajo obligatorio</b></summary>

<br>

`1` Perforar la tapa → `2` Pegar el vinilo → `3` Montar componentes

Si se pega el vinilo antes de perforar, la broca lo levanta y arruina el acabado.

**Al perforar plástico:** broca nueva, velocidad baja, y apoyar la tapa sobre madera. El plástico se raja con facilidad.

**Al atornillar:** apriete firme pero suave. El plástico cede y la LCD se pandea.

</details>

<details>
<summary><b>Distribución interna</b></summary>

<br>

| Profundidad | Contenido |
|:--|:--|
| `z = +32 a +37 mm` | Tapa: pantalla, encoder, pilotos, botón, buzzer |
| `z = +18 mm` | Arduino Mega vertical y placa de potencia |
| `z = −16 a −20 mm` | Válvulas, filtro, válvula de aguja, sensor |

**Principio:** neumática atrás, electrónica adelante. Si hay fuga o condensación, no cae sobre la electrónica.

**Punto más ajustado:** las dos válvulas suman ~80 mm de ancho sobre 116 mm útiles. Medir las válvulas reales antes de perforar. Si no caben, trasladarlas a un módulo neumático separado.

</details>

<br>

---

<br>

## `09` · Fabricación de la PCB

> Método de transferencia térmica sobre baquelita de cobre, una cara.

| ✔ | | Componente | Cant. | Función |
|:-:|:-:|:--|:-:|:--|
| [ ] | 🔴 | **Baquelita virgen de cobre** `10×15 cm` | 2 | Placa definitiva + repuesto para pruebas |
| [ ] | 🔴 | **Percloruro férrico** | 250 g | Ataque químico que retira el cobre no protegido |
| [ ] | 🔴 | **Papel transfer o fotográfico** | 5 hojas | Soporte del tóner |
| [ ] | 🟡 | **Acetona o thinner** | 1 | Limpieza del tóner tras el ataque |
| [ ] | 🟡 | **Lija 600 o lana de acero** | 1 | Preparación del cobre |
| [ ] | 🔴 | **Recipiente plástico** | 1 | Ataque químico — **nunca metálico** |
| [ ] | 🔴 | **Guantes de nitrilo y gafas** | 1 | Obligatorio |
| [ ] | 🔴 | **Brocas 1 mm y 0.8 mm** | 1 c/u | Perforación de pads |
| [ ] | 🔴 | **Cautín 30–40W, estaño y flux** | 1 | Soldadura de componentes |
| [ ] | 🟢 | **Barniz protector en aerosol** | 1 | Evita oxidación del cobre expuesto |

<details>
<summary><b>Reglas de diseño en KiCad</b></summary>

<br>

| Tipo de pista | Ancho |
|:--|:-:|
| Potencia — 12V y válvulas | **1.5 – 2 mm** |
| Señal — 5V | **0.5 mm** mínimo |

**Separación de zonas.** Potencia en la mitad superior, señal en la inferior. Las válvulas generan picos al conmutar; si sus pistas corren junto a las del sensor, ese ruido contamina la lectura de presión.

**Puentes de alambre.** Al ser una sola cara, algunos cruces no tienen solución en cobre. Dejarlos previstos en el diseño, no improvisados.

**Huellas — errores frecuentes.** JST XH es paso 2.5 mm, headers Dupont 2.54 mm, borneras KF2EDG 3.81 o 5.08 mm. Elegir la huella equivocada impide montar el componente.

**Orientación del MOSFET.** Con la cara impresa al frente y las patas abajo: `Gate` `Drain` `Source`, de izquierda a derecha. Verificar en la hoja de datos antes de rutear.

</details>

<details>
<summary><b>⚠ Impresión en espejo</b></summary>

<br>

El patrón se pega contra el cobre, por lo que debe imprimirse **invertido**.

Requiere **impresora láser**. La de inyección no sirve: el proceso depende del tóner, no de la tinta.

</details>

<details>
<summary><b>⚠ Manejo del percloruro férrico</b></summary>

<br>

- Mancha de forma **permanente** ropa, mesones y pisos
- Trabajar sobre periódico, en lugar ventilado, con guantes y gafas
- **No verter por el desagüe.** Neutralizar o llevar a punto de residuos químicos
- El proceso sale mal las primeras veces — por eso hay dos baquelitas en la lista

</details>

<br>

---

<br>

## Cronograma

| Sem. | Actividad | ✔ |
|:-:|:--|:-:|
| `1` | Pedir sensor HX710B — 2 a 3 semanas de envío | [ ] |
| `1` | Comprar electrónica base y etapa de potencia | [ ] |
| `2` | Montar protoboard y programar ciclo respiratorio con LEDs | [ ] |
| `3` | Menú de LCD y lectura de encoder | [ ] |
| `3–4` | Esquemático y ruteado en KiCad | [ ] |
| `4` | Fabricar, atacar y perforar la PCB | [ ] |
| `5` | Comprar y montar la neumática | [ ] |
| `6` | Integrar sensor y calibrar contra manómetro de la clínica | [ ] |
| `7` | Gabinete, vinilo y acabado | [ ] |
| `8` | Pruebas integradas y ajuste de parámetros | [ ] |
| `9–10` | **Margen para imprevistos — no comprometer** | [ ] |

<br>

---

<br>

## Limitaciones del diseño

> Decisiones deliberadas de alcance. Declararlas explícitamente en el informe demuestra criterio de ingeniería; omitirlas deja la impresión de descuido.

<table>
<tr>
<td width="50%" valign="top">

### Válvulas on/off

Restringe los modos ventilatorios a **controlados por volumen y por presión**.

No permite formas de onda variables, presión soporte ni modos asistidos.

**Trabajo futuro:** migración a válvula proporcional con control PID.

</td>
<td width="50%" valign="top">

### Volumen estimado

El equipo **no incorpora sensor de flujo**.

El volumen mostrado se calcula del tiempo de apertura, calibrado contra una medición externa. No es medición en tiempo real.

**Trabajo futuro:** integrar sensor de flujo tipo SFM.

</td>
</tr>
<tr>
<td width="50%" valign="top">

### Válvula espiratoria NC

Ante corte de energía el circuito queda **sellado**.

Un equipo clínico exigiría configuración normalmente abierta para permitir ventilación manual de rescate.

Aceptable en prototipo de laboratorio con supervisión permanente; la válvula de alivio mitiga parcialmente el riesgo.

</td>
<td width="50%" valign="top">

### Caudal espiratorio limitado

El orificio de 2.5 mm restringe la salida del aire y puede generar **auto-PEEP** a frecuencias altas.

**Mitigación:** operar entre 10 y 12 rpm, relación I:E de 1:3 y volúmenes moderados.

</td>
</tr>
</table>

<br>

---

<br>

## Notas de seguridad

> [!IMPORTANT]
> **La válvula de alivio mecánica es independiente del software.** Es la única protección activa si el Arduino se bloquea con la válvula inspiratoria abierta. En equipos médicos, la seguridad crítica nunca depende del programa.

> [!WARNING]
> **No utilizar oxígeno** con válvulas neumáticas industriales lubricadas. Riesgo de ignición.

> [!CAUTION]
> **Etiquetar la entrada de aire** con la presión máxima admisible. Conectar el cilindro sin regulador — 150 bar — destruye válvulas y mangueras de forma inmediata.

> [!NOTE]
> **GND común obligatorio.** El negativo del Arduino y el de la fuente de 12V deben estar unidos. Sin ese punto común el circuito no conmuta, y es la falla más difícil de diagnosticar porque aparenta ser un error de programación.

> [!TIP]
> **Los 110V permanecen fuera del gabinete.** Solo entran 12V a través del jack de panel. Esto elimina el riesgo eléctrico al abrir el equipo y es el esquema que usan monitores, bombas de infusión y otros equipos médicos pequeños.

<br>

---

<br>

<div align="center">

**Documento de trabajo** · Actualizar conforme avanza el proyecto

`VM-01` · Ingeniería Biomédica · 2026

</div>
