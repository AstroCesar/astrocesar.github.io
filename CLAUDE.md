# astrocesar.github.io — Portfolio de César Muñoz González

Portafolio personal, doble identidad: **animador 3D de personajes** (PunkRobot, Disney/Lucasfilm,
Nickelodeon) y **científico de datos / astrónomo** (35 papers peer-reviewed, surveys galácticos
como CAPOS/APOGEE). Sitio estático (HTML/CSS/JS plano, sin build step), deploy directo a GitHub
Pages. Público objetivo: reclutadores de estudios de animación y comités académicos/universitarios
— César está postulando a un puesto de animador de videojuegos en una universidad.

## Regla de oro

**Antes de tocar código, proponer 2-3 direcciones concretas y esperar aprobación.** Esto aplica a
cualquier decisión de diseño (paleta, tipografía, layout, qué mostrar). No es solo cortesía: la
motivación completa del rediseño (ver abajo) es evitar que el sitio se sienta "hecho con IA", y eso
requiere decisiones deliberadas, no defaults.

## Por qué se rediseñó (contexto, no reabrir esta discusión sin motivo)

El sitio original (dark theme, DM Serif/DM Sans/DM Mono, starfield de fondo) ya era bastante
propio, pero acumulaba patrones que leen como "portfolio generado por IA": nav flotante con
backdrop-blur, eyebrow `// portfolio personal` estilo comentario de código, badges/pills en cada
crédito, grids de "stat tiles" (dashboard con número grande + label chico), cajas de "note" con
borde de color y una palabra en negrita, texto en mayúsculas con letter-spacing en todas partes.
César teme que un reclutador entre, lo reconozca como hecho con IA en 2 segundos, y eso juegue en
contra en un puesto donde se valora el ojo/criterio visual (animación).

## Sistema de diseño actual: "Bone Ink"

Reemplaza el theme oscuro original. Paleta cálida tipo papel/tinta, tipografía editorial real
(la misma familia que usan revistas científicas), cero mayúsculas-con-tracking.

**Colores** (mismos tokens en las 4 páginas HTML):
```css
--bg: #efe9dd;        /* hueso */
--surface: #e7ded0;
--surface2: #ded3c0;
--border: rgba(33,29,23,0.14);
--text: #211d17;       /* tinta, no negro puro */
--muted: #746b5a;
--accent-anim: #7a2e2e;   /* óxido — animación */
--accent-astro: #38455c;  /* índigo desvaído — astronomía/data */
--accent-tech: #6b5a3a;   /* sepia — tech/lab (sección oculta hoy) */
```

**Tipografía** (Google Fonts):
- **EB Garamond** (500/600/700) — nombre, h1/h2, títulos de tarjetas, section-labels, sub-labels
  en itálica. Es la misma familia que usan revistas científicas reales (A&A, MNRAS) — conexión
  real con los papers de César, no una serif decorativa cualquiera.
- **IBM Plex Sans** (300/400/500) — cuerpo de texto.
- **IBM Plex Mono** (400/500) — solo donde hay datos reales (año, autores, metadata de figura).

## Reglas anti-"IA genérica" — no reintroducir estos patrones

Todo esto se sacó deliberadamente. Si se agrega contenido nuevo, seguir estas reglas:

1. **Nada de `text-transform:uppercase` + `letter-spacing`** en labels/eyebrows/nav. Si necesita
   jerarquía, usar EB Garamond itálica en su lugar (ver `.sub-label`, `.hero-eyebrow`).
2. **Nada de nav con `backdrop-filter:blur`** — fondo sólido `var(--bg)`.
3. **Nada de "stat tile" grids** (número grande en caja + label chico, en grid). Los stats se
   integran como texto corrido con el número en EB Garamond bold (`.stats-line`), no en tarjetas.
   Ya se convirtieron: publicaciones/citas en Data Science, y los dos bloques de CAPOS.
4. **Nada de "note boxes"** (caja con borde de color + palabra en negrita). Ya se convirtieron a
   párrafo plano con la palabra clave en itálica (`.note`, sin fondo/borde/padding).
5. **No inventar elementos que parezcan clickeables sin serlo.** Si algo tiene hover-state que
   sugiere interactividad, debe ser un link real. (Se corrigió "Áreas" en Data Science — eran
   `<div>` con hover que no llevaban a ningún lado.)
6. **Radios de borde:** escala chica y consistente (4-10px). Nada de `border-radius:999px` (pills).
7. **Un solo acento por color-context** — no mezclar el verde/dorado/azul-grisáceo original, esos
   quedaron reemplazados por óxido/índigo/sepia.

## Estructura de archivos

| Archivo | Qué es |
|---|---|
| `index.html` | Página principal. Todo en un archivo: `CONFIG` (bloque JS arriba, editar ahí nombre/email/links/videos), CSS inline en `<style>`, HTML, JS de fade-in al final. |
| `capos.html` | Página del survey CAPOS (astronomía), linkeada desde Data Science. |
| `scripts-3d.html` | Scripts propios de Maya/animación, linkeada desde nav "Scripts" e index. |
| `scripts-data.html` | Scripts propios de ciencia de datos, linkeada desde Data Science. |
| `unity-demo/` | Build WebGL de Unity embebido como iframe en el crédito "Animation Cycles". |

**El nav se repite en las 4 páginas HTML — si se agrega/saca un ítem, sincronizar en las 4.**

## Estado actual / pendientes conocidos

- **"Tech & Lab" está oculta** (comentada en `index.html`, sacada del nav en las 4 páginas) porque
  no tiene contenido todavía. Reactivar cuando César tenga algo que poner ahí — sacar el
  comentario `<!-- SECCIÓN 3 — TECNOLOGÍA -->` en `index.html` y devolver el `<li>` al nav en las
  4 páginas (están comentados/documentados en el propio código).
- Videos pesados (15-19MB: `BraveCat_Punkrobot.mp4`, `WowLisaCesarR.mp4`,
  `integration_animation.mp4`) sin comprimir — pendiente si el tiempo de carga en mobile molesta.
- Contacto: dos líneas directas con el email visible (`.contact-line`), no botones — un patrón
  intencional, no reemplazar por botones genéricos.

## Herramientas de diseño instaladas

Skill `design-taste-frontend` (`npx skills add Leonxlnx/taste-skill`) instalado en
`.claude/skills/` — reglas anti-slop de frontend (tipografía, color, layout, motion). Si no
aparece en la lista de skills disponibles de la sesión, leer `.claude/skills/design-taste-frontend/SKILL.md`
directamente en vez de invocarlo con la herramienta Skill.

## Cómo previsualizar

No hay build step. Para ver el sitio localmente:
```bash
python -m http.server 8080
```
y abrir `http://localhost:8080`. (Cuidado: los enlaces a `unity-demo` y algunos assets externos
asumen el dominio real `astrocesar.github.io` en producción.)
