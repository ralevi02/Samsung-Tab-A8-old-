# Guía: LineageOS en Galaxy Tab A 8.0 con S Pen (SM-P350)

Objetivo: dejar la tablet con un Android moderno y liviano (LineageOS), sin el sistema de Samsung.
El S Pen probablemente no funcione: está asumido.

> ⚠️ Todo esto **borra la tablet por completo**. Hazlo con calma, en orden, y no te saltes pasos.

---

## Por qué la vez pasada quedó con pantalla negra

Algunas P350 traen una pantalla marca **BOE**, distinta a la de la T350. Las ROMs de la T350 solo
encienden la imagen en esa pantalla si la tablet tiene el **bootloader de Android 6.0 (Marshmallow)**.
Si la tablet estaba en Android 7 (Nougat), la ROM arranca y suena, pero la pantalla no prende.

**Solución:** instalar primero el firmware oficial de Samsung en **Android 6.0.1**, y recién después
TWRP y la ROM. Y **nunca más instalar Android 7 de Samsung**.

---

## Paso 0 — Lo que necesitas

- PC con Windows.
- **Cable USB de datos**: idealmente el original. Muchos cables baratos solo cargan.
- **Tarjeta microSD**, cualquiera de 2 GB o más. Es la forma más fácil de pasar la ROM a la tablet.
- **Carga la tablet al menos 1 hora** con el cargador de pared antes de empezar. Si la batería
  se agota en medio de un flasheo, se complica todo.

### Si la tablet parece apagada o con la pantalla negra
Puede que esté encendida con la pantalla negra (por la ROM anterior). Para forzar el apagado,
mantén **Power + Home + Volumen abajo** unos 10–15 segundos hasta que se reinicie o se apague.

---

## Paso 1 — Descargas en el PC

Ponlas todas en una carpeta, por ejemplo `C:\Tablet`.

| Qué | Dónde | Notas |
|---|---|---|
| **Drivers USB de Samsung** | developer.samsung.com/android-usb-driver | Instálalos y reinicia el PC. |
| **Odin** (3.13 o 3.14) | SamFw (samfw.com) o el hilo de Odin en XDA | Es un .zip: descomprímelo. Descarga solo de sitios conocidos. |
| **Firmware oficial SM-P350 en Android 6.0.1** | samfw.com o sammobile.com | Busca **SM-P350**, versión que empiece con **P350XXU1B…** (Android 6.0.1). Una que reportan que funciona es **P350XXU1BPH2**. **NO bajes una que diga Android 7.** Si hay de tu región, mejor; si no, cualquier región sirve para el modelo WiFi. |
| **TWRP para SM-P350** en formato **.tar** | Hilo de XDA: *"[RECOVERY][TWRP 3.1.0-1] Galaxy Tab A SM-T350/355/357/550/555 SM-P350/355/550/555"* | Reportan que **TWRP 3.1.1** funciona con pantallas BOE. Si una versión te muestra pantalla negra, prueba otra. |
| **ROM: LineageOS 18.1 para SM-T350** (.zip) | Hilo de XDA de *retiredtab*: *"[UNOFFICIAL][ROM] LineageOS 18.1 Galaxy Tab A [SM-T350]"* | Android 11, la más estable. Alternativas: 19.1 (Android 12L) o 20 (Android 13), ambas *alpha*. **Lee el primer post del hilo antes de instalar.** |
| *(Opcional)* Apps de Google | Lo que recomiende el primer post del hilo de la ROM | Para Android 11 y procesador **arm (32 bits)**, no arm64. Sin ellas la tablet anda más liviana; con ellas tienes Play Store. |

**Copia a la microSD** el .zip de la ROM (y el de Google si lo vas a usar), y métela en la tablet.

> Antes de conectar la tablet, cierra **Smart Switch** o **Kies** si los tienes: chocan con Odin.

---

## Paso 2 — Entrar al modo descarga

1. Con la tablet **apagada**, mantén **Power + Home + Volumen abajo**.
2. Aparece una pantalla de advertencia → presiona **Volumen arriba** para continuar.
3. Conecta el cable USB al PC.

Esta pantalla la dibuja el bootloader de Samsung, así que **debería verse aunque la ROM anterior
dejara la pantalla negra**. Fíjate en el texto de arriba a la izquierda y anota **PRODUCT NAME**
(debería decir SM-P350) y **FRP LOCK**.

---

## Paso 3 — Instalar el firmware oficial de Android 6.0.1 con Odin

1. Descomprime el firmware. Vas a ver archivos que empiezan con `BL_`, `AP_`, `CSC_`, `HOME_CSC_`
   y quizás `CP_`. Si vienen como `.tar.md5`, está bien así.
2. Abre **Odin** como administrador (clic derecho → *Ejecutar como administrador*).
3. Con la tablet en modo descarga y conectada, Odin debe mostrar una casilla con `COM` (por ejemplo `0:[COM5]`)
   y el mensaje **Added!!**. Si no aparece, revisa la sección *Problemas* al final.
4. Carga los archivos:
   - **BL** → el archivo `BL_...`
   - **AP** → el archivo `AP_...` (es grande, Odin se demora en cargarlo; espera)
   - **CP** → el archivo `CP_...` *(solo si existe)*
   - **CSC** → el archivo `CSC_...` (**no** el `HOME_CSC`; el normal deja la tablet limpia)
5. En **Options** deja marcados solo **Auto Reboot** y **F. Reset Time**.
   **No marques Re-Partition** ni nada más.
6. Presiona **Start** y no toques nada. Al terminar debe decir **PASS** en verde.
7. La tablet se reinicia sola. El primer arranque demora varios minutos.

**Comprueba que la pantalla se ve bien en Android 6.** Si es así, vamos bien.

En la configuración inicial:
- **Conéctate a WiFi solo si hace falta**, y si te ofrece actualizar el sistema, **di que no**.
- Si te pide la cuenta de Google que tenía antes, es el bloqueo FRP: ingresa esa cuenta.
- *(Opcional)* Instala la app **DevCheck** y en *Hardware* revisa si la pantalla es **BOE**.

---

## Paso 4 — Activar opciones de desarrollador

1. *Ajustes → Acerca del dispositivo → Información de software* → toca **Número de compilación** 7 veces.
2. Ve a *Ajustes → Opciones de desarrollador*.
3. Si aparece **Desbloqueo OEM**, actívalo. En algunas versiones no aparece; si no está, sigue igual.

---

## Paso 5 — Instalar TWRP con Odin

1. Apaga la tablet y entra de nuevo al **modo descarga** (Paso 2). Conéctala.
2. Cierra y vuelve a abrir Odin.
3. En **Options**, **desmarca Auto Reboot**. Esto es importante.
4. En **AP** carga el archivo `.tar` de TWRP. Deja vacías las demás casillas.
5. **Start** → espera el **PASS**.
6. Ahora viene la parte con más maña: hay que entrar **directo** a TWRP sin dejar que arranque Android,
   porque si arranca, Samsung borra TWRP:
   - Mantén **Power + Home + Volumen abajo** hasta que la pantalla se apague.
   - **Apenas se apague**, cambia rápido a **Power + Home + Volumen arriba** y mantén hasta ver el logo de TWRP.
7. Si TWRP te pide permitir modificaciones, desliza para aceptar.

Si arrancó Android en vez de TWRP, repite este paso. Si TWRP se ve negro, prueba otra versión de TWRP.

---

## Paso 6 — Limpiar e instalar LineageOS (desde TWRP)

1. *(Opcional, recomendado)* **Backup** → guarda *Boot* y *System* en la **microSD**. Así puedes volver atrás.
2. **Wipe → Advanced Wipe** → marca **Dalvik/ART Cache, System, Data y Cache** → desliza.
   - **No marques la microSD** (Micro SDcard), que ahí está la ROM.
   - No hagas *Format Data* salvo que el hilo de la ROM lo pida: un usuario reportó bootloops con eso.
3. Vuelve a la pantalla principal → **Install** → toca *Select Storage* → elige **Micro SDcard**.
4. Elige el **.zip de LineageOS** → desliza para instalar.
5. *(Opcional)* Si vas a usar Google: vuelve a **Install** y elige el .zip de Google → desliza.
   Instálalo **antes** del primer arranque. Si no, después hay que borrar todo de nuevo.
6. **Reboot System**. El primer arranque puede demorar **5–10 minutos**. Es normal.

---

## Paso 7 — Dejarla rápida

- *Ajustes → Sistema → Opciones de desarrollador*: cambia las 3 opciones de **escala de animación** a **0.5x** o desactívalas.
- Instala pocas apps y elige versiones livianas. Tiene solo 2 GB de RAM.
- Deja al menos 2–3 GB libres de almacenamiento.
- Si no instalaste Google, puedes bajar apps con **F-Droid** o **Aurora Store**.

### Reglas para no romper nada
- **Nunca instales firmware de Samsung en Android 7 (Nougat).** Vuelve la pantalla negra.
- Para actualizar LineageOS, sigue lo que diga el hilo de XDA.

---

## Problemas comunes

| Problema | Qué hacer |
|---|---|
| Odin no muestra `COM` / no dice *Added!!* | Reinstala los drivers de Samsung, prueba otro cable y otro puerto USB (de atrás del PC si es de escritorio), cierra Smart Switch/Kies, reinicia el PC. |
| Odin dice **FAIL** | Revisa que el firmware sea de **SM-P350**, prueba otra versión de Odin, otro cable u otro puerto. Vuelve a entrar al modo descarga y repite. |
| Odin se queda pegado mucho rato | Espera al menos 10 minutos. Si no avanza, desconecta, fuerza el reinicio a modo descarga y repite. |
| Tras instalar Android 6 se queda en el logo | Entra al recovery de Samsung (Power + Home + Volumen arriba) → *Wipe data/factory reset* → *Reboot*. |
| Instalaste TWRP pero arrancó el recovery de Samsung | Android arrancó antes y borró TWRP. Repite el Paso 5 con más rapidez en el cambio de botones. |
| LineageOS arranca pero pantalla negra (otra vez) | El bootloader no es el de Android 6. Vuelve al Paso 3 y comprueba que el firmware sea **6.0.1**. |
| LineageOS se queda en la animación de arranque | Espera 15 minutos. Si sigue igual, vuelve a TWRP, repite el wipe del Paso 6 e instala de nuevo. |
| Odin muestra **Added!!** pero la tablet no hace nada | Normal: Odin espera que presiones *Start*. |

---

## Fuentes

- LineageOS 18.1 / 19.1 / 20 para SM-T350 (retiredtab) — xdaforums.com
- Reportes de usuarios con pantalla BOE y el arreglo con firmware Marshmallow — hilos de LineageOS 19.1 y crDroid para SM-T350 en XDA
- TWRP para SM-T350/P350 — xdaforums.com, hilo *"[RECOVERY][TWRP 3.1.0-1] Galaxy Tab A …"*
- Firmware oficial antiguo — samfw.com, sammobile.com
