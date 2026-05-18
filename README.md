# Bloque Imán — Radiografía Econopyme

Componente standalone para captar leads desde la home de **econopyme.com**.
Marco visual con copy + form-placeholder, listo para embeber el form de DonWeb y conectar al chat IA de la radiografía.

---

## Arquitectura — tres piezas independientes

El bloque NO arma ni el form ni el chat. Solo deja el espacio listo y define la UX/visual.

| Pieza | Quién la provee | Dónde vive |
|---|---|---|
| **Marco visual** (este repo) | Vivi | `index.html` + `styles.css` |
| **Form de captura** | DonWeb (snippet/iframe) | Slot dentro de `#iman-form-placeholder` |
| **Chat IA + dashboard** | Econopyme (ya existe) | Página `/radiografia` o función JS |

Separarlas así evita el quilombo de querer hacer todo en un mismo componente.

---

## Cómo conectar el form de DonWeb

En `index.html`, dentro del bloque marcado como **SLOT FORM**, reemplazar el `<form>` placeholder por el snippet que provee DonWeb.

**Requisitos del form en DonWeb:**

- 3 campos: `nombre`, `email`, `actividad`
- Envío de los datos a EnviaLoSimple (lista correspondiente al lead magnet)
- Al hacer submit, redirige a `/radiografia` pasando los campos por query string
  → ejemplo: `/radiografia?nombre=Vivi&email=vivi@x.com&actividad=indumentaria`

**Mantener las clases CSS** (`.iman-form`, `.iman-input`, `.iman-cta`, `.field`, `.iman-label`) para conservar el estilo de marca. Si DonWeb impone HTML rígido, copiar los estilos a sus clases.

---

## Cómo conectar el chat IA — dos opciones

### Opción A (recomendada para v1) — submit + redirect

El form hace submit a `/radiografia?nombre=X&email=Y&actividad=Z`.
En esa ruta, Econopyme:

1. Toma `actividad` como input del chat IA
2. Matchea contra uno de los 24 perfiles de rubro
3. Renderiza el dashboard de la radiografía (el que ya existe)

**Ventaja:** todo el "quilombo" del chat queda del lado del servidor de Econopyme. El bloque imán no necesita JavaScript propio.

Esta opción ya está cableada en `index.html` (`action="/radiografia" method="get"`).

### Opción B (v2 — evaluar después de medir conversión) — modal in-place

El click del CTA abre un modal con el chat IA y renderiza el dashboard sin recargar.

Requiere que Econopyme exponga el chat como función JS, por ejemplo:

```js
window.econopyme.openRadiografiaChat({ nombre, email, actividad })
```

El botón ya tiene el hook listo: `data-action="open-radiografia-chat"`.
Para activarlo, sumar un `<script>` que escuche el click y llame a la función.

---

## Paleta de marca (extraída del mockup oficial)

Definida en `styles.css` bajo `:root`. Editar ahí para ajustar.

| Variable | Valor | Uso |
|---|---|---|
| `--eco-ink` | `#0A0A0A` | texto principal, fondo del CTA |
| `--eco-text-subtle` | `#6B6B6B` | subhead, microcopy |
| `--eco-text-eyebrow` | `#7A8FB8` | eyebrow ("RADIOGRAFÍA DEL SECTOR...") |
| `--eco-text-display-soft` | `#6B7B97` | subheads grandes tipo "Econopyme resuelve" |
| `--eco-text-italic-muted` | `#B0B0B0` | itálicas decorativas muted (estilo "resuelvo", "nos eligen") |
| `--eco-bg-soft` | `#F4F6F8` | fondo del form |
| `--eco-card-bg` | `#F2F2F2` | cards del sitio |
| `--eco-bg-gray` | `#E9E9E9` | fondo header de econopyme.com |
| `--eco-border` | `#E4E7EC` | bordes de inputs |
| `--eco-accent-green` | `#7BC242` | verde lima del logo |
| `--eco-accent-green-deep` | `#0E8E5C` | verde esmeralda del dashboard radiografía |

---

## Tipografía

**Inter** (Google Fonts) — la fuente que usa el sitio de econopyme.com (Figma Sites).
Pesos cargados: 400 / 500 / 600 / 700 + itálica.

**Regla de marca para itálicas:** la itálica es énfasis en el **mismo color** del headline, no en color de acento. Sigue el patrón del home de Econopyme — "Poner orden es *simple*", "Herramientas para tomar *decisiones reales*", "Desde $19.500 por mes *final*".

Si Econopyme cambia la fuente oficial, reemplazar el `<link>` de Google Fonts en `index.html` y la variable `--eco-font-sans` en `styles.css`.

---

## Estructura de archivos

```
iman-radiografia/
├── index.html      ← bloque (copy + slot form + CTA + trust strip)
├── styles.css      ← paleta + tipografía + layout mobile-first
└── README.md       ← este archivo
```

---

## Cómo abrir, editar, versionar

1. Abrí `index.html` en el navegador → ya renderiza el bloque.
2. Para editar copy: directamente en `index.html`.
3. Para editar marca/colores: variables en `:root` de `styles.css`.
4. Para versionar en GitHub:
   ```bash
   cd iman-radiografia
   git init
   git add .
   git commit -m "v0 — bloque imán radiografía"
   git branch -M main
   git remote add origin git@github.com:<usuario>/econopyme-iman.git
   git push -u origin main
   ```

---

## Próximos pasos

- [ ] Validar paleta y tipografía con Guille (reunión semanal del viernes)
- [ ] Pegar snippet de DonWeb form en el SLOT FORM
- [ ] Confirmar que `/radiografia` acepta los query params del form
- [ ] Embeber el bloque debajo del CTA principal de la home
- [ ] Conectar EnviaLoSimple (Vivi, con acceso de Guille)
- [ ] Armar el Mail #1 de la secuencia
