# Bolsillo BCH — repositorio F-Droid

Repositorio F-Droid auto-hospedado de **Bolsillo BCH** (`bch.bolsillo.app`), servido con GitHub Pages.

- URL del repo: https://vulkan0n.github.io/fdroid/repo/
- QR: `qr.png`
- Huella de la clave del repo (SHA-256):
  `9B:64:30:59:F7:46:6E:DC:9F:29:0B:15:1F:A7:D1:ED:AA:61:A9:CB:31:A8:A2:74:45:09:CF:80:4E:C6:28:9B`

## Estructura

- `repo/` — índice firmado y APKs (esto es lo que consume el cliente F-Droid).
- `metadata/bch.bolsillo.app.yml` — metadata de la app.
- `index.html` — landing del repositorio.
- `qr.png` — QR con URL + fingerprint.

## Secretos (NO se commitean)

- `keystore.p12` — clave de firma del repositorio.
- `config.yml` — contiene las passwords del keystore.

**Respaldá ambos archivos.** Si perdés `keystore.p12`, los usuarios tienen que volver a agregar el repositorio y no van a recibir actualizaciones automáticas.

## Publicar una versión nueva

1. En el repo de la app: bump `versionCode` / `versionName` en `android/app/build.gradle`.
2. `pnpm build:ci && npx cap sync android`
3. `cd android && ./gradlew clean assembleRelease`
4. Verificar: `apksigner verify --print-certs android/app/build/outputs/apk/release/app-release.apk`
5. Copiar el APK a `repo/bch.bolsillo.app_<versionCode>.apk` en este repo.
6. `fdroid update`
7. Commit y push de `repo/` (no de `config.yml` ni `keystore.p12`).

> **Importante:** `repo/diff/` también se publica. El cliente F-Droid lo usa para
> actualizaciones incrementales; si falta, el update falla con 404 en
> `repo/diff/<timestamp>.json`.

## Firma

El APK se firma con `android/bolsillo-release.keystore` (repo de la app). Es una firma distinta a la de Google Play: instalar este APK sobre una versión de Play requiere desinstalar primero.
