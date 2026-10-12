# Setup OneBlock Grid - Servidor Minecraft

Plantilla preconfigurada 100% de punta a punta de un servidor **OneBlock en Cuadrícula (Grid 12K x 12K)** en el vacío, con fronteras independientes por jugador, sistema de vidas hardcore con destierro, vouchers físicos, NPCs interactivos, economía y rangos completos sin vuelo (`/fly`).

---

## 1. Arquitectura y Rendimiento Base

* **Núcleo:** Purpur 1.20.4 (Build 2176) con Java 21 / 17.
* **Límite Global del Mundo:** `12.000 x 12.000 bloques` (`max-world-size=12000`).
* **Generación en Vacío Absoluto (*Clean Void*):** Cero bloques naturales de terreno generados, reduciendo el consumo de disco y RAM en más de un 80%.
* **Cuadrícula y Distancia entre Islas:**
  * Distancia entre centros de islas: `1.500 bloques` (`distance-between-islands: 1500`).
  * Radio de protección asignado: `750 bloques` (`protection-range: 750`), abarcando un cuadrado exacto de 1.500x1.500 bloques por jugador.
* **Borde Visual por Paquetes:**
  * Addon `Border` activado en modo `VANILLA`.
  * Los jugadores ven el WorldBorder nativo de Minecraft ajustado exactamente a sus 1.500 bloques.
  * Si un jugador intenta salir de su territorio, el sistema lo bloquea y lo teletransporta de vuelta de forma segura (`bounce-back: true`, `return-teleport: true`).

---

## 2. Dimensiones y Control de Portales

* **Overworld (`oneblock_world`):**
  * Cada jugador aparece en un bloque infinito en el centro de su cuadrícula y avanza por fases (Llanura, Subterráneo, Invierno, Océano, Selva, etc.).
* **Nether (`oneblock_world_nether`):**
  * **Habilitado** (`generate: true`, `islands: true`).
  * Al encender un portal, genera su plataforma en el vacío del Nether en las coordenadas de su isla, manteniendo el aislamiento entre jugadores.
* **The End:**
  * **Deshabilitado / Bloqueado** (`allow-end: false` en `bukkit.yml` y `generate: false` en AOneBlock).

---

## 3. Motor de Vidas y Destierro al Spawn (`OneBlockLives`)

* **Parámetros:**
  * Vidas máximas: **4**.
  * Vidas iniciales: **4**.
* **Mecánica de Muerte en OneBlock:**
  * Al morir en `oneblock_world` o el Nether, el jugador pierde 1 vida (`-1`) con alertas visuales en pantalla (`Title` y `ActionBar`).
* **Destierro Total (0 Vidas):**
  * Al llegar a 0 vidas, es teletransportado de inmediato al **Spawn** (`world`).
  * Quedan cancelados todos los comandos de isla (`/ob go`, `/ob home`, `/ob visit`).
  * Se interceptan portales y teletransportes hacia las dimensiones de OneBlock.
  * Mensaje en pantalla: *"¡Has perdido todas tus vidas! Estás atrapado en el Spawn. Consigue un Voucher de Vida para poder regresar a tu isla"*.
* **Comandos:**
  * `/vidas` (o `/lives`): Muestra el estado de vidas en corazones (`❤ ❤ ❤ ❤`).
  * `/vidas give <jugador>`: Entrega el Voucher de Vida físico.
  * `/vidas add <jugador> <cantidad>`: Suma vidas (máximo 4).

---

## 4. Vouchers Físicos de Vida (`CrazyVouchers`)

* **Ítem:** Nether Star encantada con brillo (`vida_voucher`).
* **Nombre:** `&c&lVoucher de Vida &7(+1 Vida)`.
* **Propiedades:** Ítem físico almacenable en cofres, tradeable y comerciable libremente entre jugadores.
* **Mecánica Inteligente:**
  * Al dar clic derecho, añade `+1 vida`.
  * Si el jugador ya tiene las 4 vidas completas, el sistema cancela el consumo para evitar desperdiciarlo (*"Ya tienes el máximo de 4 vidas"*).
  * Si el jugador estaba desterrado con 0 vidas, desbloquea de inmediato su acceso a OneBlock.

---

## 5. Hub del Spawn y NPCs Interactivos (`ZNPCsPlus`)

Motor de NPCs ultra ligero basado en paquetes (sin consumo de IA). Se encuentran preconfigurados 3 NPCs en el Spawn:
1. **Guardián de OneBlock (`&a&lGuardián de OneBlock`):**
   * Interactuar intenta enviar al jugador a su isla (`/ob go`). Si tiene 0 vidas, cancela y avisa que está desterrado.
2. **Mercader de Vidas (`&c&lMercader de Vidas`):**
   * Interactuar abre la interfaz para consultar o canjear Vouchers de Vida.
3. **Banquero Central (`&e&lBanquero Central`):**
   * Interactuar permite consultar el balance económico (`/balance`) y realizar transferencias (`/pay`).

---

## 6. Sistema de Rangos OneBlock (Sin Fly)

Jerarquía configurada en **LuckPerms** (almacenamiento YAML) y adaptada a OneBlock con **prohibición total de vuelo (`/fly`)**:

```
[USUARIO] ──► 🟢 ZERO ──► 🔵 HUNTER ──► 🔴 LORD ──► 👑 SCORPION
```

| Beneficio / Habilidad | [USUARIO] | 🟢 ZERO | 🔵 HUNTER | 🔴 LORD | 👑 SCORPION |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Vuelo (`/fly`)** | ❌ **PROHIBIDO** | ❌ **PROHIBIDO** | ❌ **PROHIBIDO** | ❌ **PROHIBIDO** | ❌ **PROHIBIDO** |
| **Auto-Pickup en OneBlock** | ❌ | ✅ Sí | ✅ Sí | ✅ Sí | ✅ Sí |
| **Auto-Smelt (Menas fundidas)** | ❌ | ❌ | ✅ Sí | ✅ Sí | ✅ Sí |
| **Compactar (`/condense`)** | ❌ | ❌ | ❌ | ✅ Sí | ✅ Sí |
| **Rescate Caída al Vacío** | ❌ | ❌ | ❌ | Cooldown 6h | Cooldown 3h |
| **Voucher de Vida en Kit** | ❌ | ❌ | ❌ | ❌ | ✅ 1 Mensual |
| **Límite Miembros en Isla** | 2 | 3 | 4 | 5 | 6 |
| **Hogares (`/sethome`)** | 1 | 2 | 3 | 5 | 8 |
| **Color del WorldBorder** | Azul | Azul | Azul | Personalizable | Dorado/Personalizable |
| **Comandos Portátiles** | Ninguno | `/craft` | `/craft`, `/ec`, `/trash` | + `/anvil`, `/furnace` | + `/repair`, `/feed` |

---

## 7. Economía y Kits (Vault + EssentialsX)

* Saldo inicial para nuevos jugadores: `$500`.
* Kits automáticos por rango:
  * `/kit zero`: Herramientas de hierro, cubos de agua/lava, poción de caída lenta.
  * `/kit hunter`: Herramientas de diamante, carne, 3 pociones de caída lenta, retoños de roble.
  * `/kit lord`: Herramientas de diamante encantadas, andamios, manzanas doradas y perlas de ender.
  * `/kit scorpion`: Herramientas de Netherite encantadas, lingote de Netherite, manzanas doradas y **1 Voucher Físico de Vida (+1)**.

---

## 8. Cómo Iniciar o Desplegar

### En local (Termux / Linux / PC):
```bash
cd /storage/ED1E-BD27/OneBlock-Setup
./start.sh
```

### En Panel Pterodactyl:
1. Subir el contenido de `/storage/ED1E-BD27/OneBlock-Setup/` a la raíz del servidor.
2. Asignar `server.jar` y Java 17 o 21.
