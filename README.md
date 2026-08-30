# PDF-SV — sitio de descargas (APK + PDF)

Sitio **100% estático** (HTML/CSS/JS, sin backend) para distribuir un APK y un PDF.
Diseño tipo iLovePDF. Se publica en Netlify y se actualiza de dos formas:

- **`git push`** a `main` (lo de siempre), o
- desde el **panel privado** en una URL secreta, sin tocar la terminal.

```
descarga-app/
├── index.html                  La página pública
├── netlify.toml                Config de Netlify (MIME del APK, noindex del panel)
├── .gitignore
├── downloads/
│   ├── pdfsv.apk               <-- tu APK  (nombre exacto)
│   └── documento.pdf           <-- tu PDF  (nombre exacto)
└── panel-9k4m2xqz/
    └── index.html              Panel privado para subir APK / PDF
```

---

## 1. Probar en local

Con Laragon: `http://descarga-app.test` o abre `index.html` en el navegador.
Copia tu APK y tu PDF en `downloads/` con los nombres `pdfsv.apk` y `documento.pdf`
(o cambia los nombres en `index.html` **y** en `netlify.toml`).

---

## 2. GitHub + Netlify

- Repo: <https://github.com/edominguez84/ilovepdf>
- Netlify: proyecto **pdf-sv**, ya conectado al repo.
  Cada `git push` a `main` dispara un despliegue automático (1–2 min).
- Panel de despliegues: <https://app.netlify.com/projects/pdf-sv/deploys>

---

## 3. Actualizar el APK o el PDF

### Opción A — terminal

```bash
# reemplaza downloads/pdfsv.apk y/o downloads/documento.pdf
git add .
git commit -m "APK v1.3"
git push
```

### Opción B — panel privado (sin terminal)

URL secreta: **`https://<tu-sitio>.netlify.app/panel-9k4m2xqz/`**
(no está enlazada desde ninguna parte y lleva cabecera `noindex`).

Primera vez, configura dos cosas en `panel-9k4m2xqz/index.html`:

```js
var PANEL_PASS = "CAMBIA-ESTA-CLAVE";       // pon tu contraseña
var REPO       = "edominguez84/ilovepdf";   // ya está bien
```

Haz `git push` de ese cambio una sola vez. A partir de ahí:

1. Abre la URL secreta e introduce la contraseña.
2. Pega **una vez** un *fine-grained token* de GitHub con permiso
   **Contents: Read and write** sobre el repo `ilovepdf`
   (<https://github.com/settings/personal-access-tokens/new>).
   El token se guarda **solo en tu navegador** (localStorage); no se sube al sitio.
3. Arrastra el `.apk` o el `.pdf` y pulsa *Publicar*.
   El panel hace un commit en `main` y Netlify redespliega solo.

> **Por qué el token no va escrito en la página:** el sitio servido es público
> (cualquiera con la URL vería el token en el código y GitHub lo revocaría al
> instante). Pegándolo una vez por navegador queda igual de cómodo y sin ese riesgo.

---

## Tamaño del APK

- **< 100 MB:** funciona por ambas vías (terminal y panel).
- **> 100 MB:** GitHub lo rechaza. Usa **GitHub Releases**: sube el APK como
  *asset* de un Release y cambia el `href` del botón en `index.html` por esa URL.

---

## Personalizar

- Nombre y textos: edita `index.html` (busca `PDF-SV`).
- Color principal: variable CSS `--red` al inicio de `<style>`.
- Icono: SVG embebido en `<link rel="icon">`.
