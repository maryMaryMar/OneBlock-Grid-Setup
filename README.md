# Setup OneBlock Grid - Servidor Minecraft

Plantilla preconfigurada lista para desplegar de un servidor **OneBlock en Cuadrícula (Grid)** con fronteras independientes por jugador, sistema de parties y dimensiones optimizadas.

---

## 1. Arquitectura del Servidor

* **Núcleo:** Purpur 1.20.4 (Build 2176) con Java 21 / 17.
* **Mundo en el Vacío:** No genera terreno natural, reduciendo el consumo de disco y memoria en más de un 80%.
* **Cuadrícula y Distancia entre Islas:**
  * Distancia entre centros de islas: `500 bloques` (`distance-between-islands: 500`).
  * Radio de protección asignado: `250 bloques` (`protection-range: 250`), abarcando un cuadrado exacto de 500x500 bloques.
* **Borde Visual / Virtual:**
  * Addon `Border` activado en modo `VANILLA`.
  * Los jugadores ven el WorldBorder nativo de Minecraft ajustado exactamente a su radio de 250 bloques.
  * Si un jugador intenta salir de su territorio, el sistema lo bloquea y lo teletransporta de vuelta de forma segura (`bounce-back: true`, `return-teleport: true`).

---

## 2. Dimensiones

* **Overworld (`oneblock_world`):**
  * Cada jugador aparece en un bloque infinito en el centro de su cuadrícula. Al romperlo, progresa a través de las fases (Llanura, Subterráneo, Invierno, Océano, Selva, etc.).
* **Nether (`oneblock_world_nether`):**
  * **Habilitado** (`generate: true`, `islands: true`).
  * Al construir un portal al Nether, cada jugador genera su propia isla/plataforma en el vacío del Nether en sus coordenadas protegidas, sin chocar con otros jugadores.
* **The End:**
  * **Deshabilitado / Bloqueado** (`allow-end: false` en `bukkit.yml` y `generate: false` en AOneBlock). Los portales del End no se activarán hasta que se decida abrir esta fase.

---

## 3. Cooperativo y Equipos (Parties)

Los jugadores pueden jugar en solitario o fusionarse con amigos usando los comandos integrados de BentoBox / AOneBlock:
* `/ob invite <jugador>`: Invita a otro jugador a unirse a la isla. Al aceptar, ambos comparten el mismo OneBlock y área de 500 bloques.
* `/ob accept / /ob reject`: Acepta o rechaza invitaciones.
* `/ob trust <jugador>`: Otorga permisos de construcción temporal a un visitante sin que abandone su propia isla.
* `/ob team`: Menú de gestión de miembros, rangos y permisos de la isla.

---

## 4. Plugins Instalados

| Plugin / Addon | Función | Ubicación |
| :--- | :--- | :--- |
| **Purpur (server.jar)** | Motor de alto rendimiento con optimizaciones de chunks y ticks. | `/` |
| **BentoBox** | Motor central de gestión de islas, cuadrícula y protecciones. | `plugins/` |
| **AOneBlock** | Addon que transforma la isla en el bloque evolutivo OneBlock por fases. | `plugins/BentoBox/addons/` |
| **Border** | Addon del WorldBorder por paquetes para delimitar los 500 bloques. | `plugins/BentoBox/addons/` |
| **Level** | Cálculo de nivel de isla y ranking de bloques colocados. | `plugins/BentoBox/addons/` |
| **Vault** | API de economía para compatibilidad futura de tiendas. | `plugins/` |
| **EssentialsX** | Comandos esenciales, spawn, economía básica y utilidades. | `plugins/` |

---

## 5. Cómo Iniciar o Desplegar

### En local (Termux / Linux / PC):
```bash
cd /storage/ED1E-BD27/OneBlock-Setup
./start.sh
```
*(Puedes editar la variable `RAM="2G"` en `start.sh` según la memoria disponible del sistema)*.

### En un Host / Panel Pterodactyl:
1. Comprimir o subir todo el contenido de `/storage/ED1E-BD27/OneBlock-Setup/` a la raíz del servidor.
2. Asegurarse de que el archivo de inicio apunte a `server.jar` y Java 17 o 21.
