# Game Save Sync

**[Abrir el generador →](https://frozenfrostfox.github.io/game-save-sync/)** · [Read in English](README.md)

Mantené las partidas guardadas de tus juegos sincronizadas entre tu PC, tu Android (celular o
consola portátil) y Linux, usando **tu propio Google Drive**: sin servidores y sin cuentas en
ningún lado más que Google.

Completás un formulario (nombre del juego, dónde están las partidas, qué carpeta de Drive usar) y
la página te genera un script listo para tu plataforma. Abrís el juego a través de ese script: baja
la partida más nueva de Drive, abre el juego, espera a que salgas y sube tu progreso.

> La interfaz del generador está en inglés; esta guía te explica cada paso en español.

## Qué hace

| Plataforma | Script que genera | Cómo sincroniza |
|---|---|---|
| 🪟 **Windows** | Python (`.py`, se puede empaquetar como `.exe`) | Piloto automático: sincroniza, abre el juego, lo vigila, vuelve a sincronizar cuando lo cerrás y se cierra solo. |
| 🤖 **Android** | Bash para [Termux](https://termux.dev) + rclone | Un toque desde un widget en la pantalla de inicio (Termux:Widget). Detecta solo hacia qué lado copiar. |
| 🐧 **Linux** | Bash + rclone | Automático como en Windows si le das el nombre del proceso del juego; si no, apretás Enter cuando terminás de jugar. |

- **Gana la más nueva:** cada sincronización compara las fechas de modificación local y de Drive y copia en la dirección correcta.
- **Carpeta o archivos:** sincroniza una carpeta de partidas entera, o uno o varios archivos puntuales.
- **Reimportar:** ¿perdiste la configuración? Cargá un script (o un `.exe` de Windows que hizo la página) y el formulario se completa solo.
- **Un solo comando para Termux:** la pestaña de Android copia un comando que escribe el script directo en Termux.
- **Privado:** el generador es un único archivo HTML. Nada de lo que escribís sale de tu navegador; tus credenciales de Google solo se guardan en el almacenamiento local de tu navegador.

## Cómo empezar

1. Abrí el **[generador](https://frozenfrostfox.github.io/game-save-sync/)** (o descargá `index.html` y abrilo en tu compu: funciona sin internet).
2. Creá en Drive la carpeta donde van a ir las partidas y copiá su enlace.
3. Elegí tu plataforma y hacé la configuración de una sola vez que aparece en la barra lateral:
   - **Windows:** Python 3.8+, `pip install google-api-python-client google-auth-oauthlib` y un cliente OAuth gratis de Google Cloud (`credentials.json`, unos 5 minutos; los pasos están en la barra lateral).
   - **Android:** Termux desde F-Droid, `termux-setup-storage`, `pkg install rclone coreutils findutils` y después `rclone config` para Google Drive.
   - **Linux:** instalá rclone y corré `rclone config` para Google Drive.
4. Completá el formulario, descargá el script y de ahora en más abrí el juego con él.

¿Querés una app de doble clic en Windows? El generador también te da un `build_<juego>_exe.bat`
que instala PyInstaller y arma `sync_<juego>.exe` por vos.

### Para tener en cuenta

- En Android 11 o más nuevo, las apps no pueden leer los datos privados de otras sin root, así que las partidas de juegos nativos de Android solo funcionan si el juego guarda en el almacenamiento compartido. Las de emuladores casi siempre están ahí.
- Windows puede bloquear un `.exe` descargado (SmartScreen / "Mark of the Web"). Clic derecho → Propiedades → **Desbloquear**, o `Unblock-File` en PowerShell.
- ¿No sabés dónde guarda un juego? [PCGamingWiki](https://www.pcgamingwiki.com) tiene la ubicación de casi todos los juegos de PC.

## Cómo se hizo

Proyecto personal, construido de a poco con asistentes de IA (Claude y ChatGPT) y usado todos los
días para mantener sincronizadas las partidas entre varias PCs y consolas portátiles con Windows,
un Android y Linux. La lógica del generador se probó con pruebas en Node.js y chequeos de sintaxis
de `bash -n` y Python antes de cada versión.

## Licencia

[GPL-3.0](LICENSE) © 2026 FrozenFrostFox. Podés usarlo, modificarlo y compartirlo; las versiones
que distribuyas tienen que seguir siendo código abierto con la misma licencia.
