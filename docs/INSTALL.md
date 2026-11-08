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

### Step 3 — Install the core (required, once)

```bash
ssh root@YOUR-RECALBOX-IP
```

```bash
mount -o remount,rw /
cp /recalbox/share/system/configs/retroarch/cores/anygm_libretro.so /usr/lib/libretro/
cp /recalbox/share/system/configs/retroarch/cores/anygm_libretro.info /usr/lib/libretro/
chmod 755 /usr/lib/libretro/anygm_libretro.so
killall emulationstation
```

### Play

Open **GameMaker (AnyGM)** → **Maldita Castilla**.

If the system is missing: START → update game lists.

If the game fails after a full reboot, repeat Step 3 (overlay may drop the core).

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

### Paso 2 — Logo del tema (recomendado)

Copia `anygm.png` y `anygm.xml` desde `package/tema_recalbox-next/` a las rutas del tema `recalbox-next` (y `recalbox-240p` si lo usas). Ver tabla en inglés arriba.

### Paso 3 — Instalar el core (obligatorio, una vez)

Mismos comandos SSH del apartado inglés (sustituye la IP por la tuya).

### Jugar

Sistema **GameMaker (AnyGM)** → **Maldita Castilla**.
