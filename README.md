# Bloque Imán — Radiografía Econopyme

Componente standalone para captar leads desde la home de **econopyme.com**.
Marco visual con copy + widget real de EnvíaloSimple, listo para conectar con el chat IA de la radiografía.

---

## Arquitectura — tres piezas independientes

El bloque no arma el chat ni la radiografía. Define la experiencia de entrada, captura el lead y deriva a la página donde Econopyme continúa el flujo.

| Pieza | Quién la provee | Dónde vive |
|---|---|---|
| **Marco visual** | Vivi | `index.html` + `styles.css` |
| **Form de captura** | EnvíaloSimple | Widget dentro de `#iman-form-envialosimple` |
| **Chat IA + dashboard** | Econopyme | Página `/radiografia` |

---

## Form real de EnvíaloSimple

El bloque usa este widget:

```html
<script type="text/javascript" src="https://v3.envialosimple.com/form/renderwidget/format/widget/AdministratorID/202168/FormID/1/Lang/es"></script>
```

**Alcance del form en esta versión:**

- Captura `nombre`.
- Captura `email`.
- Guarda el contacto en EnvíaloSimple.
- Redirige a `/radiografia` si esa redirección está configurada en el panel.

La `actividad` o `rubro` no se pide en este formulario. Se pide después, dentro del chat de radiografía.

---

## Conexión con el chat IA — opción v1

Flujo esperado:

1. La persona deja nombre + email en el bloque.
2. EnvíaloSimple guarda el contacto.
3. EnvíaloSimple redirige a `/radiografia`.
4. El chat de Econopyme pregunta a qué se dedica la persona.
5. Con esa respuesta, Econopyme matchea contra los perfiles de industria y renderiza la radiografía.

**Ventaja:** el bloque queda simple y no necesita JavaScript propio para interpretar el rubro.

---

## Pendiente de validar en EnvíaloSimple

- Lista/audiencia destino del lead magnet.
- Campo de nombre correctamente mapeado.
- Redirección posterior al submit hacia `/radiografia`.
- Mensajes de éxito/error.
- Si el doble opt-in queda activo o no.

---

## Paleta de marca

Definida en `styles.css` bajo `:root`.

| Variable | Valor | Uso |
|---|---|---|
| `--eco-ink` | `#0A0A0A` | texto principal, CTA |
| `--eco-text-subtle` | `#6B6B6B` | subhead, microcopy |
| `--eco-text-eyebrow` | `#7A8FB8` | eyebrow |
| `--eco-bg-soft` | `#F4F6F8` | fondo del form |
| `--eco-border` | `#E4E7EC` | bordes |
| `--eco-accent-green` | `#7BC242` | verde lima del logo |
| `--eco-accent-green-deep` | `#0E8E5C` | verde dashboard |

---

## Tipografía

**Inter** vía Google Fonts.

Regla visual: la itálica del headline usa el mismo color del texto, no color de acento.

---

## Estructura de archivos

```txt
iman-radiografia/
├── index.html
├── styles.css
├── README.md
└── .gitignore
```

---

## Próximos pasos

- [ ] Probar el widget real con un email controlado.
- [ ] Confirmar que el contacto entra en la lista correcta.
- [ ] Configurar redirección a `/radiografia`.
- [ ] Confirmar que el chat pide actividad/rubro al entrar.
- [ ] Embeber el bloque en la home real.
- [ ] Configurar Mail #1 en la automatización.
