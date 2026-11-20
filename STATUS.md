# STATUS — Fork «Tony» v1 (offline, iPhone)

Fecha: 2026-11-20 · Carpeta: `~/workspace/tony-app-ios/`
Base: PocketPal AI (MIT), rama `main`, commit
`ed0b9654b3be0123a36712c0923dc3d6d41cac21` (2026-11-19), versión 1.24.0.
El código se obtuvo como tarball oficial de esa rama (el `git clone`
directo se interrumpió en esta VM; el contenido es el del commit citado).

## Cambios exactos y archivos tocados

### 1. Rebrand a «Tony»
- `app.json` — `displayName`: «Tony». **El campo `name` se queda
  «PocketPal» a propósito**: es la clave de registro del componente JS y
  debe coincidir con `withModuleName: "PocketPal"` en
  `ios/PocketPal/AppDelegate.swift` y con `getMainComponentName()` en
  Android. Cambiarlo sin tocar nativo rompe el arranque; el nombre
  *visible* va por displayName + CFBundleDisplayName.
- `ios/PocketPal/Info.plist` — `CFBundleDisplayName`: **Tony** (nombre
  bajo el icono en iOS) y las 6 descripciones de permisos
  (cámara/micrófono/red local/ubicación/fotos/Siri) ahora dicen «Tony».
- `ios/PocketPal.xcodeproj/project.pbxproj` — bundle id del target de la
  app (3 configuraciones): `ai.pocketpal` → **`com.josedaniel.tony`**.
  El target de tests conserva `ai.PocketPalTests`.
- `ios/PocketPal/PocketPal.entitlements` — grupo de keychain alineado al
  nuevo bundle id: `$(AppIdentifierPrefix)com.josedaniel.tony`.
- `android/app/src/main/res/values/strings.xml` — `app_name`: Tony
  (cosmético; el applicationId Android no se tocó, esta v1 es iOS).
- `src/locales/en.json` — 6 textos visibles con la marca: splash
  (`brand: Tony`), «Welcome to Tony», descripción de pantalla sin modelos,
  dos mensajes de permiso de cámara, título de notificación de voz.
  (Regla del repo: solo se edita `en.json`; las demás locales vienen de
  Weblate.) Quedan textos «PocketPal AI» en pantallas de apoyo/feedback y
  en PocketPal Plus: son del proyecto original y no se reescribieron para
  no atribuir a Tony lo que es de upstream — pendiente de decisión.

### 2. Tema visual de Tony (tema oscuro, `src/theme/tokens/colors.ts`)
Fondo oscuro existente (negro) + paleta de Tony:
- `secondary` oscuro: `#C9A227` (dorado champagne), con `onSecondary`
  oscuro para contraste.
- `tertiary` oscuro: `#22D3EE` (cian, brillo del reactor).
- `primaryContainer` oscuro: `#5C1220` (rojo candy apple profundo).
- Burbuja del autor: `#2A0D13` (tinte candy apple).
- Burbuja de «pensando»: familia cian (`#22D3EE` sobre `#0B2530`).
- Botón primario: familia dorada (fondo `#241D08`, borde `#6B5716`,
  texto `#C9A227`).
- Icono de modelo de texto: `#22D3EE`.
El tema claro queda como viene de upstream (Tony es oscuro por diseño).

### 3. Cara de Tony
- Fuente: `~/workspace/avatars/avatar-1792606183708500713-0.webp`
  (1600×1600), convertida a PNG con Pillow.
- `src/assets/tony-avatar.png` (1024×1024) — imagen nueva; la usan
  `src/components/ChatEmptyPlaceholder/ChatEmptyPlaceholder.tsx` y
  `src/components/VideoPalEmptyPlaceholder/VideoPalEmptyPlaceholder.tsx`
  en lugar de `pocketpal-dark-v2.png` (los estilos ya eran cuadrados con
  esquinas redondeadas: 112px y 96px, calzan con la cara).
- Icono de la app: `AppIcon.png`, `AppIcon-dark.png` y
  `AppIcon-tinted.png` (esta última en escala de grises, como pide iOS
  para el modo tintado) regenerados a 1024×1024 desde el avatar en
  `ios/PocketPal/Images.xcassets/AppIcon.appiconset/`.

### 4. Persona por defecto (prompt de sistema, en español)
Texto exacto encargado, puesto en los dos lugares donde upstream define
su pal por defecto («Pip» → «Tony»):
- `src/store/PalStore.ts` — el pal sembrado al primer arranque
  (`initializePipPal`): nombre «Tony», descripción en español, persona
  íntegra, color `['#3D060F', '#C9A227']`, y la clave de siembra pasa a
  `PalStore.builtin.Tony.seeded`.
- `src/store/onboarding/onboardingPals.ts` — el pal de onboarding
  (key interna `pip` intacta: el mapa de temas y la copia l10n cuelgan de
  ella): nombre «Tony», misma descripción/persona/color, saludo en español
  con 4 prompts sugeridos en español.

### 5. Modelo recomendado: Qwen3-4B Q4_K_M (verificado en la web)
Verificado contra el repo oficial **Qwen/Qwen3-4B-GGUF** (Hugging Face,
API oficial, 2026-11-20):
- Archivo: `Qwen3-4B-Q4_K_M.gguf` — 2.497.280.256 bytes — 4.022.468.096
  parámetros.
- En `onboardingPals.ts`, Tony ofrece 3 niveles: rápido = LFM2.5 1.2B
  (el que traía upstream), **recomendado (preseleccionado) = Qwen3-4B
  Q4_K_M del repo oficial**, mejor = Qwen3-4B Q5_K_M del mismo repo
  (2.889.513.184 bytes, verificado).

### 6. Licencia y atribución
- `LICENSE` — **idéntico byte a byte** al de upstream (verificado contra
  el tarball original).
- `NOTICE.md` — nuevo: atribución del fork, base y commit, cambios y
  marcas de terceros.

### 7. Compilación en la nube
- `.github/workflows/build-ios-unsigned.yml` — **nuevo** (el repo traía
  `release.yml`, que sube a TestFlight con fastlane y secretos de pago;
  no servía para un .ipa sin firmar). El nuevo flujo, de disparo manual,
  replica los pasos iOS de upstream (Xcode del runner `macos-26`,
  Node 22.21.0, `yarn install`, arreglo de enriched-markdown,
  Ruby 3.2.3, `pod install`, `Env.xcconfig`/`.env` generados — estos
  últimos con PalsHub/autenticación **desactivados** y modo offline
  activado) y termina con `xcodebuild archive` con la firma
  completamente deshabilitada + empaquetado `Payload/*.app` →
  artefacto descargable **`tony-ios-unsigned-ipa`** (`Tony-unsigned.ipa`).

### 8. Documentos
- `README-TONY.md` — qué es, cómo generar el .ipa en la cuenta
  `jdnewman2212-ux`, cómo instalar con SideStore desde Windows, firma
  gratis de 7 días y renovación, primera descarga del modelo y uso
  offline, límites honestos.
- `STATUS.md` — este archivo.

## Verificación realizada (en esta VM Linux)
- ✅ Sintaxis TypeScript/TSX de los 5 archivos de código editados
  (`PalStore.ts`, `onboardingPals.ts`, `colors.ts`,
  `ChatEmptyPlaceholder.tsx`, `VideoPalEmptyPlaceholder.tsx`):
  transpilación con TypeScript 5.6 standalone sin errores.
- ✅ `app.json` y `src/locales/en.json`: JSON válido.
- ✅ `Info.plist`: plist válido (plistlib), `CFBundleDisplayName = Tony`.
- ✅ Workflow nuevo: YAML válido, job y trigger correctos.
- ✅ `LICENSE` idéntico al original; archivos núcleo no tocados
  (`package.json`, `src/utils/chat.ts`, `ios/Podfile`) idénticos.
- ✅ Datos del modelo Qwen3-4B verificados vía API oficial de Hugging
  Face (repo, nombre de archivo, tamaño y parámetros exactos).
- ✅ Imágenes generadas: dimensiones y archivos comprobados en disco.
- ⚠️ `yarn install` intentado dos veces (105 s c/u): la descarga de
  paquetes se quedó en reintentos de red en esta VM y no completó; por
  eso **no se corrieron `yarn lint`, `yarn typecheck` ni los tests de
  jest** (las reglas del repo los piden antes de un PR; este paquete no
  se subió a ningún PR). Al correrlos en el primer build/CI pueden salir
  ajustes: los tests de upstream que fijan el pal «Pip» (p. ej. en
  `PalStore`/onboarding) y los snapshots de componentes con el tema
  oscuro necesitarán actualización a «Tony» y a los nuevos colores.
- ❌ **El build iOS no se puede hacer en esta VM** (Linux, sin Xcode):
  la primera compilación real es la del flujo de GitHub Actions.

## Bloqueos y pendientes
**Del lado del usuario:**
1. Crear el repo privado en `jdnewman2212-ux` y subir esta carpeta
   (no se subió nada desde aquí, por instrucción).
2. Correr el flujo «Build Tony iOS (unsigned IPA)» y descargar el .ipa.
3. Instalar SideStore en la laptop Windows + iPhone y firmar/instalar
   (firma gratis de 7 días; renovar desde SideStore).
4. Primera apertura con internet: completar el onboarding (tema general)
   y descargar **Qwen3 4B (recomendado)** (~2,5 GB); contexto en 4096.
5. Probar en el equipo real (micrófono/voz, memoria con el modelo
   cargado) y reportar: es la primera verificación en dispositivo.

**Técnicos / conocidos:**
- Funciones en línea de upstream (PalsHub, cuentas, PocketPal Plus,
  Google Sign-In, Firebase) quedan desactivadas o sin credenciales en
  esta build; con el bundle id nuevo y la firma de SideStore tampoco
  funcionarían tal cual. El chat local no depende de ellas.
- El esquema/target de Xcode y el nombre interno del módulo siguen
  llamándose «PocketPal» (decisión deliberada, ver §1): por dentro el
  .app se genera como `PocketPal.app` dentro del .ipa; el nombre visible
  en iOS es Tony.
- Tema claro sin rebranding de color y restos de marca PocketPal en
  pantallas secundarias (ver §1): pulido pendiente si la v1 funciona.
- Tests de upstream desactualizados respecto al rebrand (ver arriba).
