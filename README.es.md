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
- **Privado:** el generador es un único archivo HTML. Nada de lo que escribís sale de tu navegador; mirá [Privacidad](#privacidad).

## Cómo empezar

1. Abrí el **[generador](https://frozenfrostfox.github.io/game-save-sync/)** (o descargá `index.html` y abrilo en tu compu: funciona sin internet).
2. Creá en Drive la carpeta donde van a ir las partidas y copiá su enlace.
3. Elegí tu plataforma y hacé la configuración de una sola vez que aparece en la barra lateral:
   - **Todos:** hacé la [configuración de Google de una sola vez](#configuración-de-una-sola-vez-tu-propio-inicio-de-sesión-de-google-5-minutos-gratis) de abajo y pegá tu `credentials.json` en la página.
   - **Windows:** Python 3.8+ (marcá *Add python.exe to PATH* al instalarlo).
   - **Android:** Termux desde F-Droid, `termux-setup-storage`, `pkg install rclone coreutils findutils` y después pegá el comando de una línea para Google Drive que te da la página.
   - **Linux:** instalá rclone y pegá el comando de una línea para Google Drive que te da la página.
4. Completá el formulario, sacá el script y de ahora en más abrí el juego con él.

¿Querés una app de doble clic en Windows? Tocá **Build .exe (copy command)**, abrí PowerShell en la
carpeta donde la querés y pegá. Instala PyInstaller y arma `sync_<juego>.exe` ahí. No se descarga
nada, así que Windows no marca nada como inseguro. También podés descargar un
`build_<juego>_exe.bat`, que por defecto se borra solo cuando termina bien.

## Configuración de una sola vez: tu propio inicio de sesión de Google (~5 minutos, gratis)

Los scripts necesitan permiso para usar tu Google Drive. Ese permiso se da con un "cliente OAuth"
gratis que creás una sola vez en tu propia cuenta de Google. Nadie más lo ve.

1. Entrá a [console.cloud.google.com](https://console.cloud.google.com/) y creá un proyecto (por ejemplo *Cloud Save Sync*).
2. **APIs & Services → Library** → buscá *Google Drive API* → **Enable**.
3. **Google Auth Platform → Get started**: nombre de la app, tu email como email de soporte, Audience **External**, tu email como contacto → **Create**.
4. **Data Access → Add or remove scopes** → pegá `https://www.googleapis.com/auth/drive` en *Manually add scopes* → **Add to table** → **Update** → **Save**.
5. **Branding**: completá *Application home page* y *Application privacy policy link* (la URL de esta página sirve para los dos) y agregá el dominio del sitio en *Authorized domains* (por ejemplo `frozenfrostfox.github.io`) → **Save**. Sin esto Google no te deja publicar.
6. **Clients → Create client** → Application type **Desktop app** → **Create** → **Download JSON**. Google muestra el secreto una sola vez, así que descargalo antes de cerrar el cuadro.
7. **Audience → Publish app → Confirm**. ⚠️ No te saltees esto: mientras la app esté en *Testing*, Google te cierra la sesión cada 7 días.
8. Abrí el `client_secret_….json` descargado con el Bloc de notas, copiá todo y pegalo en el generador (paso 5 de Windows). Hacelo aunque solo uses Android o Linux: la página lo usa para armar tu comando de configuración de rclone. La página lo recuerda solo en tu navegador.

La primera vez que corre un script, Google muestra *"Google no verificó esta app"*. Es tu propia
app, así que hacé clic en **Configuración avanzada → Ir a (nombre de tu app)**. Se hace una sola vez por dispositivo.

### Para tener en cuenta

- En Android 11 o más nuevo, las apps no pueden leer los datos privados de otras sin root, así que las partidas de juegos nativos de Android solo funcionan si el juego guarda en el almacenamiento compartido. Las de emuladores casi siempre están ahí.
- Windows marca los `.bat`/`.exe` descargados (SmartScreen / "Mark of the Web"). Usá el armado con el comando copiado para evitarlo, o clic derecho en el archivo → Propiedades → **Desbloquear**, o `Unblock-File` en PowerShell.
- Si un script dice que tu sesión de Google venció, abre el navegador para que vuelvas a entrar automáticamente. Si te pasa todas las semanas, tu app de Google sigue en *Testing*: hacé el paso 7 de la configuración.
- ¿No sabés dónde guarda un juego? [PCGamingWiki](https://www.pcgamingwiki.com) tiene la ubicación de casi todos los juegos de PC.

## Privacidad

- El generador es una página estática. Nada de lo que escribís se manda a ningún lado; tu `credentials.json` se guarda solo en el almacenamiento local de tu navegador.
- Los scripts hablan solo con Google. Tu sesión de Google se guarda solo en tu propio dispositivo, al lado del script. Tus partidas van solo a tu propio Google Drive.
- No hay servidor, ni estadísticas, ni cuenta con nadie más que Google. Para quitar el acceso, borrá el archivo de sesión que está al lado del script, o quitá la app en [myaccount.google.com/permissions](https://myaccount.google.com/permissions).

## Cómo se hizo

Proyecto personal, construido de a poco con asistentes de IA (Claude y ChatGPT) y usado todos los
días para mantener sincronizadas las partidas entre varias PCs y consolas portátiles con Windows,
un Android y Linux. La lógica del generador se probó con pruebas en Node.js y chequeos de sintaxis
de `bash -n` y Python antes de cada versión.

## Licencia

[GPL-3.0](LICENSE) © 2026 FrozenFrostFox. Podés usarlo, modificarlo y compartirlo; las versiones
que distribuyas tienen que seguir siendo código abierto con la misma licencia.
