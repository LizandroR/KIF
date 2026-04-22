# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Proyecto

Landing page estática para **KIF**, agencia de marketing digital con base en Los Ríos y La Araucanía (Chile). Una sola ruta, sin backend, sin build step. El CTA principal lleva a conversaciones de WhatsApp con Felipe y Katherine (los socios).

## Arquitectura

Todo el sitio vive en un único archivo `index.html` autocontenido (~1450 líneas) con CSS y JS inline. **No hay build, no hay dependencias, no hay package.json.** Esta decisión es deliberada — ver la sección "Decisiones de diseño".

### Secciones de la página (en orden de aparición)
Nav → Hero → About → Method (timeline) → Offer (servicios) → Plans → Work (testimonios) → Contact → Footer + FAB flotante.

### Sistema de diseño
Tokens CSS en `:root` (y override en `[data-theme="dark"]`). La paleta completa está en los vars:
- `--coral: #FF6B7A` — acentos, CTAs, badge "popular"
- `--violet: #7B7FF5` — cards principales, plan integral
- `--navy: #1E1B5E` — fondo sección servicios, texto sobre amarillo
- `--yellow: #F5B942` — plan inicial, acentos sobre violeta

**Tipografía:** Clash Display (headlines) + Satoshi (UI/body) desde Fontshare, JetBrains Mono (números técnicos) desde Google Fonts. Cualquier cambio tipográfico debe respetar este sistema de 3 fuentes.

### Convenciones específicas del código

- **Scroll reveal:** cualquier elemento con clase `.reveal` se anima al entrar al viewport vía `IntersectionObserver`. Elementos dentro de `.services`, `.plans-grid`, `.testimonials` o `.extras-list` reciben stagger automático (120ms entre hermanos).
- **Planes color-coded:** cada plan tiene su "color world" completo. Las clases `.plan.inicial` (amarillo), `.plan.featured` (coral), `.plan.integral` (violeta) controlan border, tab, precio y bullets (`li::before`). Si agregas un cuarto plan, replica el patrón completo incluyendo dark mode variants.
- **Timeline:** la barra de progreso (`#tlFill`) se llena horizontal en desktop (width) y vertical en mobile (height). La detección ocurre en JS con `window.innerWidth > 980`. Si cambias ese breakpoint, actualiza también el CSS en `@media (max-width: 980px)`.
- **Theme toggle:** usa `prefers-color-scheme` para el estado inicial, pero **no persiste** entre recargas (es in-memory por diseño). Si se pide persistencia, `localStorage` es aceptable.
- **Mockup de Instagram (`.ig-*`):** todo el prefijo `ig-` es el mockup del hero. `.phone` (sin prefijo) es el CTA de teléfono en la sección contacto — no los confundas.

## Deploy

Auto-deploy en **GitHub Pages** desde la rama `main`, raíz del repo. Cada push dispara un build nuevo en ~1-2 min.

- **URL en vivo:** https://lizandror.github.io/KIF/
- **Repo:** https://github.com/LizandroR/KIF (público)
- Forzar build manual: `gh api repos/LizandroR/KIF/pages/builds -X POST`
- Ver último build: `gh api repos/LizandroR/KIF/pages/builds/latest --jq '.status, .error.message'`

## Decisiones de diseño (context crítico)

- **No migrar a framework sin razón fuerte.** Next/React/Astro están descartados mientras sea una sola ruta. Astro es la opción si en el futuro se agregan blog o landings por servicio — no antes.
- **Sin build step.** Agregar Webpack/Vite/Tailwind CLI solo se justifica si el CSS supera los ~3000 líneas o si se divide en componentes.
- **WhatsApp antes que formulario.** El público objetivo (PyMEs locales chilenas) convierte mejor por WhatsApp. No agregar form de contacto sin pedirlo explícitamente.

## Qué falta (roadmap conocido)

Gaps identificados pendientes de implementar:
- Meta description, Open Graph tags, Twitter Card (crítico para previews en WhatsApp)
- Favicon + theme-color meta
- JSON-LD con schema `LocalBusiness` (SEO local Chile)
- `robots.txt` + `sitemap.xml`
- Analytics (Umami Cloud o GA4)
- Posible dominio propio (`.cl` o `.com`) + migración a Cloudflare Pages

## Reglas del usuario relevantes para este repo

- Código, commits y comunicación en **español**.
- Commits sin firma de IA ni co-autoría automática.
- No commitear `CLAUDE.md` a menos que se pida explícitamente.
- No crear archivos `.md` de documentación sin petición explícita.
