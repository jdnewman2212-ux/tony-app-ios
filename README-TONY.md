# Tony — tu asistente offline para el iPhone

**Qué es.** «Tony» es tu copia offline de Tony: un asistente personal que
vive dentro de tu iPhone 14 Pro Max y funciona **sin internet**, pensado
para cuando estés de viaje o sin señal. Conversa en español, redacta,
resume, traduce, calcula y planifica con un cerebro que corre en el propio
teléfono (Qwen3-4B). Nada sale del equipo: no hay cuentas, ni nube, ni
suscripción.

**De dónde sale.** Es un fork (una copia adaptada) de **PocketPal AI**, una
app de código abierto con licencia MIT que corre modelos GGUF en el
teléfono con el motor llama.cpp. La licencia original está intacta en
`LICENSE` y la atribución en `NOTICE.md`. Lo que se cambió para convertirla
en Tony está detallado en `STATUS.md`.

---

## 1. Generar el archivo instalable (.ipa) con GitHub Actions

El iPhone solo acepta apps compiladas con Xcode en una Mac. No necesitas
una: GitHub compila por ti, gratis, en sus máquinas macOS.

1. Crea un repositorio **privado** en tu cuenta de GitHub
   (**jdnewman2212-ux**), por ejemplo llamado `tony-app-ios`.
2. Sube a ese repositorio todo el contenido de esta carpeta (incluidos los
   archivos ocultos `.github/`).
3. En el repositorio: pestaña **Actions** → flujo
   **«Build Tony iOS (unsigned IPA)»** → **Run workflow**.
4. Espera a que termine (la primera vez puede tardar bastante: instala
   dependencias y compila la app completa).
5. Al final de la corrida, descarga el artefacto
   **`tony-ios-unsigned-ipa`**. Dentro viene **`Tony-unsigned.ipa`**.

Ese .ipa va **sin firmar** a propósito: el paso siguiente lo firma con tu
Apple ID gratis.

> Si el flujo falla en tu cuenta porque el runner `macos-26` no está
> disponible, edita `.github/workflows/build-ios-unsigned.yml` y cambia
> `runs-on: macos-26` por `runs-on: macos-latest`, y vuelve a correrlo.

## 2. Instalar en el iPhone con SideStore (desde tu laptop Windows)

SideStore instala apps firmándolas con tu propio Apple ID, sin cuenta de
desarrollador paga.

1. En tu laptop Windows instala **SideStore** siguiendo su guía oficial
   (sidestore.io): se instala primero su compañero en la PC, se conecta el
   iPhone por cable USB, se confía en el equipo y se inicia sesión con tu
   Apple ID. Activa también la VPN local que SideStore pide (LocalDevVPN)
   y el **Modo de desarrollador** en el iPhone (Ajustes → Privacidad y
   seguridad → Modo de desarrollador) si te lo solicita.
2. Pasa el archivo `Tony-unsigned.ipa` al iPhone (iCloud Drive, cable o la
   propia app Archivos).
3. En SideStore, importa el .ipa: lo firma con tu Apple ID y lo instala.
   En la pantalla de inicio aparecerá **Tony**, con la cara de Tony como
   icono.
4. Si al abrirla iOS pide confiar en el desarrollador: Ajustes → General →
   VPN y gestión de dispositivos → confía en tu propio Apple ID.

**La firma gratis dura 7 días.** No es que la app se borre: al vencer,
deja de abrir hasta renovarla. SideStore puede renovarla automáticamente
estando en la misma red Wi-Fi que tu laptop con su compañero activo, o con
un toque en SideStore → Refresh. La renovación es de un minuto. (La
alternativa sin renovaciones es la cuenta Apple Developer de $99/año, que
firma por un año; no hace falta para empezar.)

Límite del Apple ID gratis: máximo **3 apps** instaladas por esta vía a la
vez.

## 3. Primera vez: descargar el cerebro (con internet)

La app sin modelo es solo el cuerpo. La primera apertura hazla **con
internet**:

1. Abre Tony y completa las pantallas de bienvenida eligiendo el tema
   general («smartchat»): tu pal por defecto ya es **Tony**.
2. Cuando te ofrezca el modelo, elige **Qwen3 4B (recomendado)** —
   es el Qwen3-4B oficial en calidad Q4_K_M, **~2,5 GB** de descarga:
   - Repo: `Qwen/Qwen3-4B-GGUF` (oficial de Qwen en Hugging Face)
   - Archivo: `Qwen3-4B-Q4_K_M.gguf`
3. Espera a que descargue completo y se cargue el modelo.

Desde ese momento ya no necesitas internet para nada: **modo avión
incluido**. En tu iPhone (6 GB de RAM) deja el contexto en **4096** como te
recomendé, para que iOS no cierre la app por memoria.

Si algún día quieres cambiar el modelo a mano: Modelos → añadir desde
Hugging Face → repo `Qwen/Qwen3-4B-GGUF` → archivo
`Qwen3-4B-Q4_K_M.gguf`.

## 4. Límites honestos

- **No es el Tony de la nube.** El cerebro local es un modelo de 4B: muy
  capaz para conversar, redactar, resumir, traducir y planificar, pero
  razona por debajo de un modelo frontera grande. Es el precio de que sea
  tuyo, privado y sin internet.
- **No sabe cosas en vivo.** Sin internet no hay precios, clima ni
  noticias; su persona está escrita para decirlo con honestidad en vez de
  inventar.
- **Sin herramientas externas.** Esta versión no lee tu correo ni tu
  calendario ni controla la casa: es conversación y conocimiento local.
- **Velocidad y batería.** Generar texto usa el chip a fondo: responde a
  ritmo de lectura, calienta algo el teléfono y gasta batería en sesiones
  largas.
- **Funciones en línea de PocketPal quedan fuera o limitadas.** El fork se
  compila con PalsHub y el inicio de sesión desactivados; pueden quedar
  restos visuales de la marca PocketPal en pantallas secundarias
  (apoyo/feedback del proyecto original). Lo esencial —chat local con
  Tony— no depende de nada de eso.
- **La firma de 7 días** es el peaje de la ruta gratis; ver sección 2.

## 5. Archivos de esta carpeta

- `README-TONY.md` — este documento.
- `STATUS.md` — cambios exactos, verificación hecha y pendientes.
- `NOTICE.md` — atribución del fork (PocketPal AI, MIT).
- `LICENSE` — licencia MIT original, intacta.
- `.github/workflows/build-ios-unsigned.yml` — el flujo que genera el .ipa.
- El resto es el código de PocketPal AI con los cambios de `STATUS.md`.
