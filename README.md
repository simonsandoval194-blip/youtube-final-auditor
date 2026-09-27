# youtube-final-auditor

Skill para [Claude Code](https://docs.claude.com/en/docs/claude-code) que detecta riesgos en documentales y vídeos informativos de YouTube **durante la producción** y hace la **auditoría final antes de publicar**.

**Filosofía: detectar el riesgo cuando todavía es barato cambiarlo.** Lo ideal es no descubrir, con 10 minutos ya montados, que una imagen, una música o una recreación clave da problemas.

> ⚠️ **Aviso importante**
> Esta skill **no garantiza la monetización** ni la admisión o permanencia en el Programa de Partners de YouTube (YPP), ni que el vídeo quede **libre de reclamaciones de Content ID, retiradas por copyright o acciones de YouTube**. Tampoco es asesoramiento legal. Un 🟢 solo significa que la revisión no detectó un problema relevante con la información disponible. Las decisiones finales las toman YouTube, los titulares de derechos y, en su caso, los tribunales.

## Dos modos

### 1. Modo preventivo (en cualquier etapa)

Revisa un material concreto antes de construir sobre él: una imagen de Internet, una foto histórica, un clip, una entrevista, una captura, una música, un efecto de sonido, una imagen generada con IA, una recreación, una afirmación del guion o una fuente documental.

Ejemplos de uso:
- *"¿Puedo usar esta imagen?"*
- *"Audita estos assets antes de incorporarlos."*
- *"Revisa este fragmento del guion."*
- *"¿Hay algún problema con esta música?"*
- *"Revisa estas imágenes generadas con IA."*
- *"¿Estamos construyendo el documental sobre una base segura?"*
- *"Haz una auditoría preventiva de lo que llevamos."*

Cada material recibe uno de estos niveles:

| Nivel | Significado |
|---|---|
| 🟢 **OK** | No se identificó un problema relevante con la información disponible. |
| 🟡 **ADVERTENCIA** | Hay una incertidumbre o un riesgo que conviene resolver antes de depender del material. |
| 🔴 **BLOQUEO** | Hay un problema crítico, con evidencia, que debe resolverse antes de seguir usándolo. |

La falta de información **no** se trata como infracción. Por ejemplo, "licencia no encontrada" da 🟡 *derechos no verificados*, no 🔴. Cada alerta indica su evidencia con una de estas etiquetas: `[VERIFICADO]`, `[NO VERIFICADO]`, `[RIESGO]`, `[CONTROVERSIA]` o `[REQUIERE REVISIÓN]`. Además, dice qué la resolvería y cuánto cuesta esperar (si el material es pieza central del vídeo o no).

### 2. Modo final (antes de publicar)

```
/youtube-final-auditor
```

o *"auditoría final antes de publicar"*.

Este modo es más exhaustivo que el preventivo: revisa todo el material usado. Intenta localizar el vídeo final, el proyecto de edición, los assets, las fuentes, el dossier, el guion, la música y los efectos, los créditos y la descripción prevista. Además:
- identifica de forma explícita qué archivo de vídeo audita (duración, resolución, fecha, relación con el proyecto) y, si no puede determinarlo, lo indica;
- intenta cotejar el guion con el audio (con una transcripción o una herramienta local, si existen); si no puede, lo marca como no verificado;
- si no hay dossier de investigación, coteja directamente con las fuentes disponibles, sin tratar esa ausencia como un error.

Termina con una única decisión:

| Decisión | Significado |
|---|---|
| 🟢 **APTO PARA PUBLICAR** | No se encontró ningún problema crítico con la información disponible. |
| 🟡 **CORREGIR ANTES DE PUBLICAR** | Hay problemas que deben resolverse antes de subir el vídeo. |
| 🔴 **NO PUBLICAR** | Hay al menos un problema crítico que hace imprudente publicar esta versión. |

**Ninguno de los dos modos modifica archivos.**

## Qué revisa

1. **Copyright / derechos**: procedencia, titular y licencia de cada material; si permite uso comercial y modificaciones; atribución exigida; qué prueba existe de todo ello.
2. **Monetización / originalidad**: contenido reutilizado y contenido no auténtico (repetitivo o producido en masa); aporte propio visible. Usar IA no se considera un problema por sí mismo.
3. **IA / transparencia**: separa la asistencia creativa del contenido sintético o alterado realista, e indica cuándo corresponde activar la divulgación de contenido alterado o sintético de YouTube.
4. **Exactitud documental**: fuentes, fechas, nombres, cifras y citas. Comprueba que se mantengan separados `[HECHO]`, `[HIPÓTESIS]`, `[CONTROVERSIA]` y `[NO VERIFICADO]`.

## Políticas oficiales y criterios propios

La skill consulta la versión actual de estas páginas oficiales antes de afirmar qué exige YouTube:

- [Políticas de monetización de canales de YouTube](https://support.google.com/youtube/answer/1311392) (contenido reutilizado y no auténtico)
- [¿Qué contenido se puede monetizar?](https://support.google.com/youtube/answer/2490020) (derechos de uso comercial)
- [Divulgación de contenido alterado o sintético](https://support.google.com/youtube/answer/14328491)
- [Uso legítimo (fair use) en YouTube](https://support.google.com/youtube/answer/9783148)
- [Reclamaciones de Content ID](https://support.google.com/youtube/answer/6013276)

El resumen de políticas que incluye `SKILL.md` se redactó el 2026-09-27. Si no coincide con la página oficial, prevalece la página oficial.

La skill también aplica **criterios conservadores propios**, que **no son requisitos de YouTube** y que el informe identifica como tales:
- **K1**: rotular en pantalla las recreaciones realistas ("Recreación");
- **K2**: documentar la procedencia de cada asset, incluida la herramienta de IA, el plan y la imagen de referencia;
- **K3**: separar hechos, hipótesis, controversias y datos no verificados;
- **K4**: documentar la licencia antes de construir una escena importante sobre un asset.

## Instalación

Requiere Claude Code. Clona el repositorio en tu carpeta de skills:

```bash
# Para todos tus proyectos
git clone https://github.com/simonsandoval194-blip/youtube-final-auditor.git ~/.claude/skills/youtube-final-auditor

# O solo para un proyecto concreto
git clone https://github.com/simonsandoval194-blip/youtube-final-auditor.git .claude/skills/youtube-final-auditor
```

Recomendado:
- Acceso web en la sesión, para consultar las políticas actuales. Si no hay acceso web, el resultado lo indica.
- `ffprobe` / `ffmpeg`, para leer los metadatos del vídeo y extraer fotogramas.

## Limitaciones

- No puede ver ni oír el vídeo como un espectador. Lo que depende de la percepción se marca como no verificado, o se comprueba con fotogramas extraídos.
- No sustituye una revisión legal ni la herramienta de comprobaciones de YouTube Studio.
- No tiene acceso a YouTube Studio: la configuración de divulgación y la descripción publicada hay que confirmarlas manualmente.
- La calidad del resultado depende de la información disponible (licencias, procedencia, fuentes y guion). Lo que la skill no encuentra, no lo da por correcto, pero tampoco lo trata como infracción.
- Las rutas que no puede leer (inexistentes, no encontradas, sin permiso o con contenido insuficiente) se listan como evidencia faltante, sin convertirlas en problemas de derechos o exactitud.
- Una cadena de texto hallada dentro de un archivo binario (imagen, vídeo, PDF) no se usa como prueba de procedencia, herramienta de IA ni licencia.
- La skill está redactada en español.

## Contribuciones y feedback

Abre un *issue* para:
- cambios en las políticas oficiales de YouTube que no estén reflejados (incluye el enlace oficial);
- falsos positivos, falsos bloqueos o riesgos no detectados (describe el caso sin compartir material privado);
- mejoras de criterio o de formato.

Las *pull requests* son bienvenidas. Cualquier regla presentada como requisito de YouTube debe apoyarse en una fuente oficial de YouTube o Google, no en blogs ni artículos SEO. Los criterios propios deben quedar identificados como tales.

## Licencia

[MIT](LICENSE). La licencia cubre el contenido de este repositorio. Las políticas de YouTube enlazadas pertenecen a sus titulares.
