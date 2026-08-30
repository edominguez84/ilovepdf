# Descarga MiApp

Sitio estático (solo HTML/CSS) para distribuir un **APK** y un **PDF**.
Sin backend. Se publica en Netlify y se actualiza haciendo `git push`.

```
descarga-app/
├── index.html            La página
├── netlify.toml          Config de Netlify (tipos MIME, descarga del APK)
├── .gitignore
└── downloads/
    ├── app.apk           <-- pon aquí tu APK
    └── documento.pdf     <-- pon aquí tu PDF
```

---

## 1. Probar en local con Laragon

El proyecto ya está en `C:\laragon\www\descarga-app`.

- Con Laragon abierto: **Menu → www → descarga-app**, o entra a
  `http://descarga-app.test` / `http://localhost/descarga-app`.
- Al ser HTML puro también sirve abrir `index.html` directo en el navegador,
  pero con Laragon las rutas se comportan igual que en Netlify.

Copia tu APK y tu PDF dentro de `downloads/` con los nombres
`app.apk` y `documento.pdf` (o cambia los nombres en `index.html` y `netlify.toml`).

---

## 2. Subir a GitHub

Dentro de la carpeta del proyecto:

```bash
git init
git add .
git commit -m "Sitio de descarga inicial"
git branch -M main
```

Crea un repositorio **vacío** en https://github.com/new (sin README), y luego:

```bash
git remote add origin https://github.com/TU-USUARIO/TU-REPO.git
git push -u origin main
```

Edita en `index.html` las dos líneas con `https://github.com/TU-USUARIO/TU-REPO`
para que apunten a tu repo real (o bórralas si no quieres el enlace).

---

## 3. Conectar Netlify (despliegue automático desde GitHub)

1. Entra a https://app.netlify.com → **Add new site → Import an existing project**.
2. Elige **GitHub** y autoriza. Selecciona tu repositorio.
3. Configuración de build:
   - **Build command:** *(vacío)*
   - **Publish directory:** `.` (o `descarga-app` si el repo tiene más carpetas)
   - Netlify ya lee `netlify.toml`, así que normalmente no tocas nada.
4. **Deploy site.**

A partir de ahí, **cada `git push` a `main` dispara un despliegue** automático.
Puedes cambiar el nombre del sitio en *Site configuration → Change site name*
para tener `https://mi-app.netlify.app`.

---

## 4. Actualizar el APK o el PDF más adelante

```bash
# reemplaza downloads/app.apk y/o downloads/documento.pdf
git add .
git commit -m "APK v1.3"
git push
```

Netlify redepliega en 1–2 minutos. La página muestra sola el tamaño y la
fecha reales de cada archivo (lee las cabeceras con una petición HEAD).

---

## APK grande

- **< 50 MB:** sin problema.
- **50–100 MB:** GitHub lo acepta pero muestra un aviso. Funciona.
  Ten en cuenta que cada versión antigua queda en el historial de git y
  el repo va creciendo. Si molesta, más adelante se puede limpiar el
  historial (BFG Repo-Cleaner) o empezar un repo nuevo.
- **> 100 MB:** GitHub **rechaza** el `push`. Opciones:
  1. **GitHub Releases:** sube el APK como *asset* de un Release y cambia
     el `href` del botón en `index.html` por la URL del release. El APK
     ya no vive en el repo y no hay límite práctico.
  2. **Git LFS:** `git lfs track "*.apk"`. Netlify soporta LFS, pero hay
     que configurarlo con cuidado; para un solo archivo suele ser más
     simple la opción de Releases.

---

## Personalizar

- Nombre y textos: edita `index.html` (busca "MiApp").
- Color: variables CSS `--red` / `--red-dark` al principio de `<style>`.
- Favicon: es un SVG embebido en el `<link rel="icon">`.
