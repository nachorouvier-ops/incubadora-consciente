# Incubadora Consciente — Landing Page

Landing de venta para la comunidad privada de Skool de **Jorge Díaz Díaz**.

Stack: **HTML5 + CSS3 + Vanilla JS**. Cero frameworks. Cero build process. Un solo archivo (`index.html`) autocontenido.

---

## 🚀 Cómo correrla en local

```bash
cd "/Users/ignaciorouvier/Claude Code/jorge-landing"
python3 -m http.server 8765
# abrir http://localhost:8765
```

> **No abras `index.html` con doble click.** El `<video>` y los `loading="lazy"` no funcionan vía `file://` en Chrome.

---

## 📁 Estructura

```
jorge-landing/
├── index.html          ← todo: HTML + CSS + JS
├── README.md
└── assets/             ← video e imágenes
    ├── hero-video.mp4   (✅ ya cargado)
    ├── 01-casco-llamas.jpg
    ├── 02-suzuki-azul-curva.jpg
    ├── 03-paddock-walkie.jpg
    ├── 04-aprilia-paddock.jpg
    ├── 05-aprilia-curva.jpg
    ├── 06-revista-boxer-cup.jpg
    ├── 07-revista-le-mans.jpg
    ├── 08-podio-jarama-trofeo.jpg
    ├── 09-loreal-curva-1.jpg
    ├── 11-yamaha-humo.jpg
    ├── 12-podio-abrazo.jpg
    ├── 13-kawasaki-lluvia.jpg
    ├── 14-loreal-curva-2.jpg
    ├── 15-kawasaki-69-detalle.jpg
    ├── 16-cev-2006-card.jpg
    ├── 17-yamaha-34.jpg
    ├── 18-pov-kawasaki-verde.jpg
    └── 19-mama-kawasaki-19.jpg
```

> **Importante:** Mientras una imagen no exista, el `.img-frame` muestra un placeholder elegante con el nombre del asset. Cuando la subís con el nombre correcto, levanta sola (no hay que tocar el HTML).

---

## ⚙️ Configuración rápida

### Cambiar el countdown

El countdown arranca en 3 días desde la primera visita de cada usuario (se guarda en `localStorage`). Para usar una **fecha fija para todos**, abrí el `<script>` y reemplazá:

```js
const COUNTDOWN_DAYS = 3;
const STORAGE_KEY = 'jdd_countdown_target';
let storedTarget = localStorage.getItem(STORAGE_KEY);
if (!storedTarget) {
  storedTarget = String(Date.now() + (COUNTDOWN_DAYS * 24 * 60 * 60 * 1000));
  localStorage.setItem(STORAGE_KEY, storedTarget);
}
const COUNTDOWN_TARGET = Number(storedTarget);
```

Por:

```js
const COUNTDOWN_TARGET = new Date('2026-12-31T23:59:59').getTime();
```

### Estrategia de precio (importante)

**El precio NO aparece en la landing.** Decisión deliberada: la página solo señala "Precio promocional de lanzamiento · cierra en [countdown]" en el banner sticky superior. El valor exacto se ve dentro de Skool, después de clickear cualquier CTA. Esto:
- Reduce fricción de objeción de precio antes de leer la propuesta
- Califica al lead (quien hace click ya quiere conocer el precio)
- Mantiene el control del checkout en Skool

Si en algún momento querés volver a mostrar el precio en la landing:
1. Schema.org Product offer → reintroducir `"price": "297"` y `"priceCurrency": "USD"` (líneas ~72 del JSON-LD)
2. Volver a poner el bloque `.pricing__brandmark` como `.pricing__amount` con el número 297
3. Sumar el valor al CTA del hero ("Quiero entrar — 297 USD") y demás
4. Actualizar la respuesta de FAQ #3 con el valor explícito

Find global por `Quiero entrar a la Incubadora` te lleva a los 4 CTAs principales.

### Cambiar el link de Skool

URL actual:

```
https://www.skool.com/jorge-diaz-transformacion
```

Aparece en **7 ubicaciones**: 5 CTAs (hero + pilares-footnote + pricing + CTA final), footer y `sameAs` del schema.org Person.

### Cambiar el poster del video hero

En el `<video>` del hero, reemplazá `poster="assets/18-pov-kawasaki-verde.jpg"` por la imagen que quieras usar como frame de carga.

### Estructura de las secciones (en orden)

1. **Banner sticky** con countdown — `aside.ticker`
2. **Hero** con video, dual CTA — `section.hero`
3. **Un espacio para transformarte** — 3 bloques alternados (`.transform-block`)
4. **El pensamiento aparece sin avisar** — narrativa de problema (`.narrative`)
5. **Tu vida no cambia haciendo más** — narrativa invertida (`.narrative.is-reverse`)
6. **Esta comunidad es para vos si…** — filtro de avatar + anti-avatar (`.audience`)
7. **Cinco pilares · Un solo camino** — el método (`#pilares`, `.pilar`)
8. **Banner-quote tipográfico** — separator (`.banner-quote`)
9. **Qué pasa cuando entrenás** — 3 pasos numerados (`.step`)
10. **Testimonios + social-proof row** — 4 cards reales (`.testimonial`)
11. **Soy Jorge** — bio editorial (`.about`)
12. **Pricing card 297 USD** — `#pricing`
13. **Garantías** — 3 íconos (`.guarantee`)
14. **FAQ** — 9 preguntas, accordion accesible (`.faq__item`)
15. **CTA final emocional** — fondo `19-mama-kawasaki-19.jpg` (`.cta-final`)
16. **Footer** con disclaimer médico

---

## 🚢 Deploy

### Opción 1 — Vercel (recomendado, gratis)

```bash
cd "/Users/ignaciorouvier/Claude Code/jorge-landing"
npx vercel --prod
```

Seguí los prompts. La primera vez te pide login.

### Opción 2 — Netlify Drop (más simple)

1. Comprimí la carpeta entera (`index.html` + `assets/` + `README.md`).
2. Arrastrá el zip a https://app.netlify.com/drop
3. Te da una URL pública en segundos.

### Opción 3 — Cloudflare Pages

```bash
npx wrangler pages deploy "/Users/ignaciorouvier/Claude Code/jorge-landing"
```

---

## 🎨 Sistema de diseño aplicado

- **Tipografía:** Fraunces (display, variable opsz 9–144) + Instrument Sans (body, variable). Decisión: las skills `frontend-design` y `ui-ux-pro-max` desaconsejan fuentes overused (Inter, Roboto). Fraunces aporta carácter editorial; Instrument Sans tiene tracking humanista sin caer en "developer mono".
- **Color:** paleta cool azulada (`oklch(11% 0.018 262)` ≈ `#0A0E1A`) con dorado cálido (`#D4A574`) y rust (`#C03B2B`) como contrapunto. El contraste cold/warm refuerza el concepto "del asfalto al cielo".
- **Spacing:** rhythm 4/8pt, fluid con `clamp()`. Secciones ≥96px desktop, ≥64px mobile.
- **Motion:** ease-out cuartic (entrada), ease-in (salida), 200/350/600ms. Stagger reveal de 80ms entre elementos.
- **Accesibilidad:** WCAG AA mínimo, contraste verificado, focus rings visibles, skip-link, `prefers-reduced-motion` respetado, video con poster + control mute, FAQ con `aria-expanded` + `aria-controls`, touch targets ≥44px.
- **Performance:** sin librerías externas (cero JS de terceros, cero CSS framework), `font-display: swap`, lazy loading bajo el fold, video con `preload="metadata"`, OKLCH nativo, sin Tailwind CDN.
- **SEO:** schema.org `Person` + `Product` + `FAQPage` con JSON-LD, OG + Twitter cards, title 60 chars, meta description con keywords del nicho, canonical, theme-color, robots.

---

## ✅ Checklist antes de publicar

- [ ] Verificar que las **18 imágenes** + `hero-video.mp4` estén en `/assets/`
- [ ] Si querés countdown fijo en lugar de "3 días desde primera visita", aplicar el cambio del script (ver arriba)
- [ ] Confirmar que el link de Skool sigue siendo `jorge-diaz-transformacion` (o actualizar si cambió)
- [ ] Reemplazar los `href="#"` del footer (Aviso legal / Privacidad / Cookies / Contacto) por URLs reales
- [ ] Subir `og-image.jpg` 1200×630 a `/assets/` (o ajustar el path en `<meta property="og:image">`)
- [ ] Actualizar `<link rel="canonical">` con la URL final de producción
- [ ] Probar video hero en Chrome, Safari, Firefox (autoplay muted loop en iOS Safari requiere `playsinline`)
- [ ] Probar en móvil (375px) y desktop (1440px)
- [ ] Verificar que los CTAs abren Skool en pestaña nueva (target="_blank" rel="noopener")
- [ ] Test de accesibilidad: Tab navigation, screen reader del FAQ y del mute
- [ ] Test de `prefers-reduced-motion`: animaciones desactivadas
- [ ] Lighthouse: Performance ≥90, Accessibility ≥95, SEO ≥95
- [ ] Test del schema.org en https://search.google.com/test/rich-results
