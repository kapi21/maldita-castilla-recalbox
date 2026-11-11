# Maldita Castilla en Recalbox 10 (Raspberry Pi 5)

Guía sencilla: **solo tienes que copiar archivos a carpetas**.  
No hace falta programar.

Probado en Recalbox **10.1.1** para Pi 5 (`rpi5_64`). Sirve también en cabinas RGB JAMMA con Recalbox.

Guía bilingüe más corta: [`docs/INSTALL.md`](docs/INSTALL.md).

---

## Qué vas a conseguir

En EmulationStation aparecerá un sistema llamado **GameMaker (AnyGM)** y dentro el juego **Maldita Castilla**.

---

## Qué necesitas

1. Recalbox 10 en una **Raspberry Pi 5**.
2. El PC y la Recalbox en la **misma red Wi‑Fi o cable**.
3. El ZIP de este paquete (ya descomprimido) o el clon del repositorio.
4. Un programa para SSH (opcional pero recomendado):
   - Windows: [WinSCP](https://winscp.net/) o la terminal de Windows con `ssh`
   - Contraseña por defecto de Recalbox: usuario `root`, contraseña `recalboxroot`

---

## Paso 0 — Abrir la carpeta compartida de Recalbox

En el PC (Windows):

1. Abre el Explorador de archivos.
2. En la barra de dirección escribe una de estas (prueba la primera):

   ```
   \\RECALBOX\share
   ```

   Si no abre, usa la IP de **tu** Recalbox (la verás en la pantalla de la consola o en el menú de red), por ejemplo:

   ```
   \\192.168.X.X\share
   ```

   Sustituye `192.168.X.X` por la IP real de **tu** máquina. No copies una IP de otro manual: cada casa/red es distinta.

Cuando se abra, verás carpetas como `roms`, `bios`, `system`, etc. Esa carpeta `share` es donde vamos a copiar casi todo.

---

## Paso 1 — Copiar el contenido de `share` (lo fácil)

Dentro del ZIP / repo hay una carpeta **`package/share`** (en el ZIP de release a veces aparece como `share`).

1. Abre esa carpeta `share`.
2. Selecciona **todo lo de dentro** (`roms`, `bios`, `system`).
3. Cópialo dentro de `\\RECALBOX\share` (o `\\TU-IP\share`).
4. Si Windows pregunta si quieres fusionar carpetas → **Sí**.
5. Si pregunta si quieres reemplazar archivos → **Sí** (salvo que tengas un `systemlist.xml` tuyo con otros sistemas; en ese caso lee el aviso de abajo).

### Aviso importante: `systemlist.xml`

El archivo va a:

```
share\system\.emulationstation\systemlist.xml
```

- Si **no** tenías ese archivo → perfecto, se crea solo.
- Si **ya** tenías un `systemlist.xml` con otros sistemas personalizados → **no lo pises**. Abre el tuyo con el Bloc de notas y añade dentro de `<systemList>` el bloque del sistema `anygm` (está en ese mismo XML del paquete). Si no sabes editar XML, pide ayuda o haz antes una copia de seguridad del archivo.

Con esto ya tienes:

| Qué | Dónde queda |
|-----|-------------|
| Juego | `roms\anygm\MalditaCastilla.droid` |
| Lista del juego + carátulas/vídeo | `roms\anygm\` y `roms\anygm\media\` |
| Config del core | `bios\AnyGM.ini` |
| Sistema en el menú | `system\.emulationstation\systemlist.xml` |
| Copia de seguridad del core | `system\configs\retroarch\cores\` |
| Logo global SVG | `system\resources\logos\anygm.svg` |

---

## Paso 2 — Logo y colores del sistema (tema)

Sin esto el juego puede funcionar, pero el sistema puede verse feo o con aspecto genérico.

Copia estos 2 archivos (carpeta `package/tema_recalbox-next` o `tema_recalbox-next` en el ZIP):

| Archivo | Destino en la Recalbox |
|---------|------------------------|
| `anygm.png` | `.../themes/recalbox-next/data/arts/systems_logos/anygm.png` |
| `anygm.xml` | `.../themes/recalbox-next/_views/_partials/systems/anygm.xml` |

Ruta completa típica (tema oficial **recalbox-next**):

```
/recalbox/share_init/system/.emulationstation/themes/recalbox-next/data/arts/systems_logos/anygm.png
/recalbox/share_init/system/.emulationstation/themes/recalbox-next/_views/_partials/systems/anygm.xml
```

Cómo copiarlos si Samba no te deja entrar en `share_init`:

1. Abre **WinSCP** (o similar).
2. Conéctate a la IP de tu Recalbox: usuario `root`, contraseña `recalboxroot`.
3. Navega a esas carpetas y sube los dos archivos con esos nombres exactos.

Opcional (temas que usan `${system.logo}`): por SSH, tras el Paso 1:

```bash
mount -o remount,rw /
mkdir -p /recalbox/system/resources/logos
cp /recalbox/share/system/resources/logos/anygm.svg /recalbox/system/resources/logos/anygm.svg
chmod 644 /recalbox/system/resources/logos/anygm.svg
```

---

## Paso 3 — Instalar el core (obligatorio, una vez)

El motor del juego (`anygm_libretro.so`) tiene que estar también en una carpeta del sistema que **no se puede escribir solo con Samba**. Hay que hacerlo una vez por SSH.

### Con WinSCP + terminal, o con `ssh` en Windows

1. Conéctate:

   ```
   ssh root@IP-DE-TU-RECALBOX
   ```

   Ejemplo: si tu IP es `192.168.1.50`, sería `ssh root@192.168.1.50`.  
   Contraseña: `recalboxroot` (no se ve al escribirla; es normal).

2. Pega estos comandos **uno tras otro** y pulsa Enter:

```bash
mount -o remount,rw /
cp /recalbox/share/system/configs/retroarch/cores/anygm_libretro.so /usr/lib/libretro/
cp /recalbox/share/system/configs/retroarch/cores/anygm_libretro.info /usr/lib/libretro/
chmod 755 /usr/lib/libretro/anygm_libretro.so
```

**Importante:** no uses `killall emulationstation`. En cabinas RGB JAMMA / CRT ese reinicio de EmulationStation puede dejar el menú **sin audio**.

En su lugar:

1. Si aparece el aviso **GAMELIST MODIFIED** → elige **Update**.
2. O menú **START** → actualizar listas de juegos.
3. O reinicia la Pi por completo: `reboot`.

---

## Paso 4 — Jugar

1. En EmulationStation busca el sistema **GameMaker (AnyGM)**  
   (no está dentro de **Ports**).
2. Entra y lanza **Maldita Castilla**.
3. Controles habituales en cabina/mando: **B** = ataque, **A** = salto, palanca = mover, Start = menú.

Si no aparece el sistema o el juego:

- Menú **START** → opciones de listas / gamelists → **Update game lists** (Actualizar listas).
- Comprueba que no tienes el sistema oculto.

---

## Si tras apagar, reiniciar o actualizar Recalbox no arranca el juego

A menudo el core en `/usr/lib/libretro/` se pierde (overlay de solo lectura).  
Vuelve a hacer **solo el Paso 3** (los mismos `cp` / `chmod`).  
Si el logo del sistema desapareció, repite también la copia opcional del SVG del Paso 2.  
El juego, el tema y la config en `share` sí suelen quedarse guardados.

---

## Problemas frecuentes

| Problema | Qué mirar |
|----------|-----------|
| No abre `\\RECALBOX\share` | Usa `\\IP-DE-TU-RECALBOX\share`. Activa Samba en Recalbox si hace falta. |
| No aparece **GameMaker (AnyGM)** | Falta `systemlist.xml` o hay que actualizar listas. |
| Aparece el sistema pero el juego falla | Falta el Paso 3 (core en `/usr/lib/libretro/`). |
| Lista negra / no se lee el nombre | Faltan los archivos del **Paso 2** (tema). |
| Menú sin sonido tras reiniciar ES | No uses `killall emulationstation`; haz **Update** o `reboot`. |
| Contraseña SSH | Por defecto: `recalboxroot`. |

---

## Estructura del paquete (resumen)

```
maldita-castilla-recalbox/
├── LEEME.md                 ← este mismo manual
├── docs/INSTALL.md          ← guía EN + ES
├── package/
│   ├── share/               ← copiar DENTRO de \\RECALBOX\share\
│   │   ├── roms/anygm/      ← juego + medias + gamelist
│   │   ├── bios/AnyGM.ini
│   │   └── system/
│   │       ├── .emulationstation/systemlist.xml
│   │       ├── configs/retroarch/cores/   ← core (respaldo)
│   │       └── resources/logos/anygm.svg  ← logo global
│   └── tema_recalbox-next/  ← logo + colores (Paso 2)
│       ├── anygm.png
│       └── anygm.xml
```

Con el Paso 1 + Paso 2 + Paso 3 queda instalado.
