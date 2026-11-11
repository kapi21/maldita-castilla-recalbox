# Install guide / Guía de instalación

English and Spanish. Full steps for Recalbox 10 on Raspberry Pi 5.

---

## English

### What you need

1. Recalbox **10.x** for **Raspberry Pi 5** (`rpi5_64`)
2. PC and Recalbox on the same network
3. This repository (or a Release ZIP)
4. SSH client (Windows: WinSCP or `ssh`). Default Recalbox login: user `root`, password `recalboxroot`

### Step 1 — Copy `package/share` into Recalbox share

On Windows, open:

```
\\RECALBOX\share
```

If that fails, use `\\YOUR-RECALBOX-IP\share` (use **your** IP).

Copy the contents of `package/share/` into that folder (merge when asked).

This installs:

- game + media → `roms/anygm/`
- `bios/AnyGM.ini`
- `system/.emulationstation/systemlist.xml`
- core backup → `system/configs/retroarch/cores/`
- global system logo → `system/resources/logos/anygm.svg`

**If you already have a custom `systemlist.xml`**, do not overwrite it blindly. Merge the `anygm` `<system>` block into your file.

### Step 2 — Theme logo (optional but recommended)

Copy from `package/tema_recalbox-next/`:

| File | Destination |
|------|-------------|
| `anygm.png` | `…/themes/recalbox-next/data/arts/systems_logos/anygm.png` |
| `anygm.xml` | `…/themes/recalbox-next/_views/_partials/systems/anygm.xml` |

Typical full paths (also copy into `recalbox-240p` if you use that theme):

```
/recalbox/share_init/system/.emulationstation/themes/recalbox-next/data/arts/systems_logos/anygm.png
/recalbox/share_init/system/.emulationstation/themes/recalbox-next/_views/_partials/systems/anygm.xml
```

Samba may not allow writing `share_init`; use WinSCP/SSH if needed.

Optional (stock themes that use `${system.logo}`): after Step 1, also copy the SVG into the live logo folder once:

```bash
mount -o remount,rw /
mkdir -p /recalbox/system/resources/logos
cp /recalbox/share/system/resources/logos/anygm.svg /recalbox/system/resources/logos/anygm.svg
chmod 644 /recalbox/system/resources/logos/anygm.svg
```

### Step 3 — Install the core (required, once)

```bash
ssh root@YOUR-RECALBOX-IP
```

```bash
mount -o remount,rw /
cp /recalbox/share/system/configs/retroarch/cores/anygm_libretro.so /usr/lib/libretro/
cp /recalbox/share/system/configs/retroarch/cores/anygm_libretro.info /usr/lib/libretro/
chmod 755 /usr/lib/libretro/anygm_libretro.so
```

Do **not** run `killall emulationstation` (on some RGB JAMMA / CRT setups that restarts ES without audio). Prefer one of these:

1. If EmulationStation shows **GAMELIST MODIFIED** → choose **Update**.
2. Or from the menu: **START** → update game lists.
3. Or reboot the Pi completely: `reboot`.

### Play

Open **GameMaker (AnyGM)** → **Maldita Castilla**.

If the system is missing: START → update game lists.

### After reboot or Recalbox OS upgrade

The core in `/usr/lib/libretro/` often disappears (read-only overlay). Repeat **Step 3** (same `cp` / `chmod` commands). Optionally re-copy `anygm.svg` into `/recalbox/system/resources/logos/` as in Step 2. Files under `share/` (game, media, configs) usually stay.

---

## Español

### Qué necesitas

1. Recalbox **10.x** para **Raspberry Pi 5** (`rpi5_64`)
2. PC y Recalbox en la misma red
3. Este repositorio (o el ZIP de Releases)
4. Cliente SSH (Windows: WinSCP o `ssh`). Por defecto: usuario `root`, contraseña `recalboxroot`

### Paso 1 — Copia `package/share` a la carpeta share

En Windows abre:

```
\\RECALBOX\share
```

Si no abre, usa `\\IP-DE-TU-RECALBOX\share` (la IP de **tu** máquina).

Copia el contenido de `package/share/` dentro (fusionar si pregunta).

Instala también el logo global: `system/resources/logos/anygm.svg`.

**Si ya tienes un `systemlist.xml` propio**, no lo pises: fusiona el bloque `<system>` de `anygm`.

### Paso 2 — Logo del tema (recomendado)

Copia `anygm.png` y `anygm.xml` desde `package/tema_recalbox-next/` a las rutas del tema `recalbox-next` (y `recalbox-240p` si lo usas). Ver tabla en inglés arriba.

Opcional: copia también el SVG a `/recalbox/system/resources/logos/` con los comandos del apartado inglés.

### Paso 3 — Instalar el core (obligatorio, una vez)

```bash
ssh root@IP-DE-TU-RECALBOX
```

```bash
mount -o remount,rw /
cp /recalbox/share/system/configs/retroarch/cores/anygm_libretro.so /usr/lib/libretro/
cp /recalbox/share/system/configs/retroarch/cores/anygm_libretro.info /usr/lib/libretro/
chmod 755 /usr/lib/libretro/anygm_libretro.so
```

**No** uses `killall emulationstation` (en cabinas RGB JAMMA / CRT puede reiniciar ES sin audio). Mejor:

1. Si sale **GAMELIST MODIFIED** → **Update**.
2. O START → actualizar listas de juegos.
3. O `reboot` completo de la Pi.

### Jugar

Sistema **GameMaker (AnyGM)** → **Maldita Castilla**.

### Tras reinicio o actualización de Recalbox

Repite el **Paso 3** (el core en `/usr/lib/libretro/` suele borrarse del overlay). Si hace falta, vuelve a copiar `anygm.svg` al logo del sistema.
