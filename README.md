# 🎪 The Amazing Digital Circus — Landing page

## Stack
- Astro · Vue 3 · Tailwind CSS v4 · pnpm

## Setup
```bash
pnpm install
pnpm dev        # http://localhost:4321
pnpm build
```

## Estructura
```
src/
├── layouts/BaseLayout.astro       # HTML base, fuentes, meta tags
├── components/
│   ├── AmbientBg.astro            # Estrellas, focos, confetti (puro CSS)
│   ├── TVScreen.astro             # TV con slot de contenido
│   └── Countdown.vue              # Cuenta regresiva reactiva (Vue)
├── pages/
│   ├── index.astro                # Página 1 — solo la TV
│   └── show.astro                 # Página 2 — info del show
└── styles/global.css              # Tokens de color y animaciones
public/images/tv-circo-digital.jpg # ← PON TU IMAGEN AQUÍ
```

## Calibrar la pantalla de la TV

Abre `src/components/TVScreen.astro` y ajusta `.tv-screen`:
```css
.tv-screen {
  top:    16%;   /* borde superior de la pantalla */
  left:   14%;   /* borde izquierdo */
  width:  72%;   /* ancho */
  height: 68%;   /* alto */
}
```
Usa DevTools (inspeccionar elemento) para afinar los valores exactos.

## Cambiar contenido de la pantalla

En `src/pages/index.astro` dentro de `<TVScreen>`:
```astro
<!-- GIF teaser -->
<img src="/images/teaser.gif" style="width:100%;height:100%;object-fit:cover" />

<!-- Video -->
<video autoplay muted loop playsinline style="width:100%;height:100%;object-fit:cover">
  <source src="/videos/teaser.mp4" type="video/mp4" />
</video>
```

## Paleta
| Variable          | Hex       |
|-------------------|-----------|
| --circus-red      | #e74d3c   |
| --circus-orange   | #f39c12   |
| --circus-yellow   | #f1c40e   |
| --circus-green    | #2ecc70   |
| --circus-blue     | #3398db   |
| --circus-purple   | #7a00cc   |
| --circus-pink     | #ff4fa3   |
