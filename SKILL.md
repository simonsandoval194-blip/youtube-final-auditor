---
name: youtube-final-auditor
description: Detección temprana de riesgos y auditoría final para documentales y vídeos informativos de YouTube, en cuatro áreas — copyright/derechos, monetización/originalidad, IA/transparencia y exactitud documental. MODO PREVENTIVO en cualquier etapa: revisa un asset, imagen, foto histórica, clip, entrevista, música, efecto de sonido, imagen de IA, recreación, fuente o fragmento de guion antes de construir sobre él, y emite 🟢 OK / 🟡 ADVERTENCIA / 🔴 BLOQUEO. MODO FINAL (gate antes de publicar): revisa vídeo, proyecto, assets, fuentes, dossier, guion, audio, créditos y descripción, y emite 🟢 APTO / 🟡 CORREGIR / 🔴 NO PUBLICAR. Usar ante "¿puedo usar esta imagen/música?", "audita estos assets", "revisa este fragmento del guion", "revisa estas imágenes de IA", "¿estamos construyendo sobre una base segura?", "auditoría preventiva", "auditoría final", "¿está listo para publicar?" o /youtube-final-auditor. Solo audita: no modifica nada.
---

# YouTube Final Auditor

**Filosofía: detectar el riesgo cuando todavía es barato cambiarlo.**
Dos modos: **preventivo** (durante la producción, material a material) y **final** (gate antes de publicar, exhaustivo).

Reglas comunes:
- **Solo audita.** No modifica, mueve, renombra ni sustituye nada. Si el usuario pide registrar las alertas en un archivo, se hace aparte y solo entonces.
- **No promete** monetización ni ausencia de reclamaciones. 🟢 = "no encontramos un problema relevante con la información disponible".
- **No es asesoramiento legal.** Las decisiones sobre derechos y fair use las toman titulares, YouTube y, en su caso, tribunales.

## 0. Elegir modo

| Petición | Modo |
|---|---|
| Un material, un lote de assets, un fragmento de guion, "¿puedo usar…?", "¿hay algún problema con…?", "¿vamos sobre una base segura?", "auditoría preventiva de lo que llevamos" | **Preventivo** (§4) |
| `/youtube-final-auditor` sin más contexto, "auditoría final", "¿está listo para publicar?", "gate final" | **Final** (§5) |

Si hay duda, preguntar en una línea. El modo preventivo nunca emite APTO / NO PUBLICAR: eso es exclusivo del modo final.

## 1. Políticas oficiales (consultar la versión actual)

Antes de afirmar qué exige YouTube, abrir las páginas oficiales con la herramienta de lectura web disponible y anotar la fecha de consulta. En modo preventivo basta con abrir las páginas relevantes para la consulta. Si no hay acceso web, usar el resumen de abajo y decirlo en el resultado ("política no re-verificada"). Solo fuentes oficiales de YouTube/Google (support.google.com, blog.youtube); nunca blogs SEO como autoridad. Si la página difiere del resumen, manda la página.

| ID | Política | URL |
|----|----------|-----|
| P1 | Monetización de canales (contenido reutilizado + contenido no auténtico, antes "repetitivo"; renombrado 15-jul-2025) | https://support.google.com/youtube/answer/1311392 |
| P2 | Qué contenido se puede monetizar (derechos comerciales de todos los elementos visuales y de audio) | https://support.google.com/youtube/answer/2490020 |
| P3 | Divulgación de contenido alterado o sintético | https://support.google.com/youtube/answer/14328491 |
| P4 | Uso legítimo (fair use) en YouTube | https://support.google.com/youtube/answer/9783148 |
| P5 | Reclamaciones de Content ID | https://support.google.com/youtube/answer/6013276 |

Resumen operativo (redactado el 2026-09-27; puede estar desactualizado, re-verificar):
- P1: reutilizado = reutilizar material ajeno "sin añadir comentario original significativo, modificaciones sustanciales o valor educativo/entretenimiento". No auténtico = producido en masa, con plantilla, poca variación entre vídeos, replicable a escala; incluye IA con plantillas genéricas sin aporte propio. **Se evalúa a nivel de canal completo.** Usar IA no es, por sí mismo, un incumplimiento.
- P2: hacen falta derechos de **uso comercial** de todos los elementos visuales y de audio. CC/royalty-free: monetizable cuando la licencia concede uso comercial. Permiso de terceros: explícito, por escrito y que cubra uso comercial.
- P3: divulgar cuando contenido **realista** está alterado o generado de forma significativa: personas reales diciendo/haciendo lo que no hicieron, alteración de metraje de hechos o lugares reales, escenas realistas que no ocurrieron. No requiere: contenido claramente irreal/animado, retoques menores (color, filtros), guion/miniatura/subtítulos asistidos por IA, clonar la propia voz para la propia narración.
- P4: fair use lo deciden tribunales caso a caso; créditos o "no copyright intended" no lo crean. Que "podría aplicar" no es garantía.
- P5: una reclamación de Content ID no equivale a infracción ni a strike; su ausencia tampoco prueba derechos.

### Criterios propios de esta skill (NO son requisitos de YouTube)
Son criterios conservadores de la skill. Al usarlos, citarlos como **"criterio de la skill"**, nunca como regla de YouTube:
- **K1 · Rótulo en pantalla** ("Recreación", "Ilustración generada con IA") cuando una imagen sintética realista pueda confundirse con archivo histórico. P3 exige la divulgación en YouTube Studio; el rótulo en pantalla es criterio nuestro.
- **K2 · Procedencia documentada** de cada asset (y, para IA: herramienta, plan, fecha, prompt y, si hubo imagen de referencia, cuál).
- **K3 · Separación [HECHO] / [HIPÓTESIS] / [CONTROVERSIA] / [NO VERIFICADO]** en guion y rótulos.
- **K4 · Documentar la licencia** (URL, captura o recibo) antes de construir una escena importante sobre un asset.

## 2. Etiquetas de evidencia y niveles

Cada afirmación del resultado lleva una etiqueta:
- **[VERIFICADO]** — comprobado en la fuente o el archivo (decir cuál).
- **[NO VERIFICADO]** — no se pudo comprobar (falta dato, acceso o percepción audiovisual).
- **[RIESGO]** — inferencia razonada sobre un posible problema; no es un hecho.
- **[CONTROVERSIA]** — las fuentes discrepan.
- **[REQUIERE REVISIÓN]** — decisión que corresponde a una persona (p. ej., valoración de fair use, parecido con una foto real, sensibilidad).

Distinguir siempre: **problema confirmado** · **riesgo** · **información faltante** · **cuestión de revisión humana**. Nunca presentar una inferencia como hecho.

**Rutas que no se pudieron comprobar.** Registrar cada una con su estado, sin confundirlos:
- **inexistente** — se comprobó que la ruta no existe;
- **no encontrado** — se buscó y no apareció (puede existir en otro lugar);
- **existe pero no accesible** — permisos, sandbox u otra limitación (decir cuál);
- **accesible pero insuficiente** — se leyó, pero su contenido no permite verificar lo pedido (decir qué falta).

Primero se dice qué no se pudo comprobar y por qué ([NO VERIFICADO]). Una limitación de acceso **no se convierte por sí sola** en problema de copyright ni de exactitud.

**Archivos binarios (PNG, JPG, MP4, PDF, etc.).** Una cadena encontrada dentro de los bytes de un archivo binario ("xAI", "Gemini", "Grok"…) **no** es metadato, nombre de herramienta, prompt, autoría, licencia ni procedencia: puede ser una coincidencia en datos comprimidos. Para afirmar procedencia (de IA o de cualquier asset) hace falta evidencia contextual fiable: metadatos legibles con un lector adecuado, nombre o documentación de la herramienta, historial de generación, registro del proyecto, captura, archivo de procedencia o declaración del creador. Si solo hay una coincidencia textual en bytes binarios ⇒ [NO VERIFICADO] y no se usa como evidencia.

Niveles (modo preventivo; el modo final los agrega por área):
- 🟢 **OK** — no se identificó un problema relevante con la información disponible.
- 🟡 **ADVERTENCIA** — incertidumbre o riesgo que conviene resolver antes de depender del material.
- 🔴 **BLOQUEO** — problema crítico **con evidencia suficiente**, que debe resolverse antes de seguir usando el material.

**La falta de información no es una infracción.** "Licencia no encontrada" = 🟡 **DERECHOS NO VERIFICADOS**, no 🔴 "copyright infringido". Usar 🔴 solo con evidencia, por ejemplo:
- los términos leídos excluyen el uso previsto (licencia solo editorial / no comercial sin otro permiso, "todos los derechos reservados" sin licencia obtenida) — tras leer los términos y el contexto (versión de la licencia, permisos adicionales, si el canal monetiza);
- el titular denegó el permiso;
- plan de usar metraje ajeno como pieza principal, sin permiso y sin aporte transformador (P1/P2);
- el guion afirma como hecho algo que las fuentes verificadas contradicen, en un punto central;
- una recreación sintética realista se presenta expresamente como grabación/fotografía auténtica.

## 3. Preguntas base (ambos modos)

Para cada material responder lo que se pueda y marcar el resto como dato faltante:
1. ¿De dónde proviene? (URL, archivo, herramienta, persona)
2. ¿Quién parece ser el titular? ([VERIFICADO] o [RIESGO] si es inferencia)
3. ¿Existe licencia? ¿Está realmente documentada (texto de la licencia, no solo "lo dice una web")?
4. ¿Permite uso comercial? ¿Permite modificaciones (recorte, color, zoom, montaje)? ¿Exige atribución y cómo?
5. Fuentes y afirmaciones: ¿fuente primaria o secundaria? ¿la afirmación está respaldada? ¿es hecho, hipótesis, controversia o no verificado?
6. IA / alteración: ¿podría confundirse con material histórico real? ¿corresponde divulgación según P3? ¿aplica K1?
7. Originalidad: ¿su uso aumenta la dependencia de material ajeno o reduce el aporte propio visible (P1)?

Nunca asumir: "está en Internet = libre", "educativo = permitido", "fair use = seguro", "tengo permiso = monetizable", dominio público sin fundamento (autor, fecha, país, ley aplicable), que una herramienta de IA concede derechos comerciales (revisar términos del plan en la fecha de generación), que una licencia CC es compatible sin comprobar variante y versión. En CC: **NC** no concede uso comercial por sí misma (hace falta otro permiso del titular; valorar según el uso y si el canal monetiza); **ND** limita las adaptaciones (revisar si recorte/montaje cuentan como tal en esa versión); **BY** exige atribución en la forma indicada.

## 4. MODO PREVENTIVO

Entrada: uno o varios materiales, rutas, URLs, un fragmento de guion o "lo que llevamos". Es más ligero que el final: analiza solo lo consultado, pero con el mismo rigor de evidencia.

Pasos:
1. Identificar el material y su **uso previsto** (escena, duración, ¿es pieza central?). Si no se sabe, preguntarlo.
2. Leer lo disponible: archivo, metadatos, página de origen, licencia, fuente citada. Si el material es una imagen o vídeo local, mirarlo (fotogramas) antes de opinar sobre su aspecto.
3. Aplicar §3 y las comprobaciones del tipo de material:

| Tipo | Comprobaciones específicas |
|---|---|
| Imagen/foto de Internet, captura de pantalla | titular real vs. quien la publica; créditos "courtesy of"/"©" dentro de artículos con licencia abierta (pueden estar excluidos); marcas de agua; capturas de webs/apps/TV = material de terceros |
| Foto histórica | autor, fecha, país y ley aplicable antes de aceptar "dominio público"; si viene de Wikimedia, leer la plantilla de licencia y su fundamento |
| Clip de vídeo / material de YouTube | titular del canal ≠ titular del contenido; licencia (CC BY de YouTube, permiso); proporción y papel en el vídeo (P1); fair use = [REQUIERE REVISIÓN] |
| Entrevista | permiso del entrevistado y del productor original; si es de terceros, mismo tratamiento que un clip |
| Música / efectos de sonido | licencia de la pista y del plan en la fecha de descarga; uso comercial en YouTube; si genera Content ID (P5) no implica infracción, pero afecta ingresos; música generada con IA: términos de la herramienta y P3 si es realista |
| Imagen/vídeo generado con IA | K2 (herramienta, plan, fecha, prompt, imagen de referencia); ¿la referencia era una foto de terceros?; ¿realista y sobre hechos/personas reales? → P3 + K1; ¿replica una foto histórica concreta? → [REQUIERE REVISIÓN] |
| Recreación de un hecho real | P3 (escena realista); K1; que el guion no la presente como archivo |
| Voz sintética | términos de la herramienta; ¿imita a una persona real? (P3); si no, P3 no la exige expresamente → [RIESGO] bajo, como interpretación |
| Afirmación del guion | fuente, tipo (primaria/secundaria), clasificación K3; cifras, fechas, nombres y citas contra la fuente; ¿el dossier la marca [NO VERIFICADO]? (sin dossier: ver "Sin dossier" en §5.2-D) |
| Fuente documental | primaria/secundaria; autoría; fecha; si hay versiones contradictorias → [CONTROVERSIA] |

4. Estimar el **coste de esperar**: si el material será pieza central (escena clave, base de un bloque, música de todo el vídeo), una 🟡 sube de prioridad: conviene resolverla antes de editar.
5. Emitir el resultado. Si faltan datos que solo el usuario tiene (de dónde salió, qué plan, qué imagen se adjuntó), preguntarlos en lugar de suponerlos.

### Tipos de alerta (plantillas)
- 🟡 **ALERTA DE DERECHOS** — «Este material podría usarse para <escena>, pero no se ha encontrado una licencia comercial verificable. No significa que su uso sea necesariamente ilegal, pero no conviene construir una escena importante dependiendo de él hasta verificarlo o sustituirlo.»
- 🟡 **ALERTA DE IA** — «Parece una recreación generada con IA de un hecho real. Si se presenta como fotografía auténtica, hay riesgo de confusión. Revisar la divulgación de P3 y considerar un rótulo de recreación (K1, criterio de la skill).»
- 🟡 **ALERTA DE EXACTITUD** — «El guion presenta esta afirmación como hecho, pero las fuentes disponibles la clasifican como hipótesis/controversia. Resolver antes de incorporarla como afirmación factual.»
- 🟡 **ALERTA DE ORIGINALIDAD** — «Este tramo depende de material ajeno sin aporte propio visible. Añadir análisis, contexto o transformación, o reducir su peso (P1).»
- 🔴 **BLOQUEO** — «Los términos de <licencia/plan> [VERIFICADO] no permiten el uso comercial previsto. Sustituir o documentar una licencia compatible antes de usarlo.»

### Formato preventivo
Para un material:
```
────────────────────────────────────
YOUTUBE FINAL AUDITOR · CONSULTA PREVENTIVA
────────────────────────────────────
MATERIAL: <nombre/ruta/URL>
USO PREVISTO: <escena · duración · ¿pieza central?>
ESTADO: 🟢 OK / 🟡 ADVERTENCIA / 🔴 BLOQUEO

ALERTAS:
- <🟡/🔴 TIPO> — <texto>
  Evidencia: [VERIFICADO]/[NO VERIFICADO]/[RIESGO]/[CONTROVERSIA]/[REQUIERE REVISIÓN] <qué se observó y dónde>
  Base: <P1–P5 · criterio de la skill K1–K4 · fuente documental>
  Qué lo resolvería: <licencia a documentar, fuente a buscar, rótulo, sustituto…>

DATOS QUE FALTAN / PREGUNTAS:
-
COSTE DE ESPERAR: bajo / medio / alto — <qué depende de este material>
────────────────────────────────────
```
Para varios materiales: primero una tabla (`material · uso · estado · alerta principal`), después el bloque anterior solo para los 🟡/🔴.

Para "¿estamos construyendo sobre una base segura?" / "auditoría preventiva de lo que llevamos": revisar lo que exista (assets, guion, fuentes, proyecto) y devolver el estado por área (COPYRIGHT · ORIGINALIDAD · IA · EXACTITUD) con 🟢/🟡/🔴, las alertas priorizadas por coste de esperar y la lista de datos faltantes. Aclarar que **no es la decisión de publicación**.

## 5. MODO FINAL (gate antes de publicar)

Más exhaustivo que el preventivo: cubre **todo** el material usado, no solo lo consultado. Si existen alertas preventivas previas, comprobar si se resolvieron.

### 5.1 Localizar materiales
Buscar en la carpeta del proyecto y registrar qué se encontró y qué falta:
1. Vídeo final (mp4/mov) → duración, resolución, pistas de audio (p. ej. con ffprobe, si está disponible). Identificarlo de forma explícita (ver abajo).
2. Proyecto de edición (CapCut, Premiere Pro, DaVinci Resolve, Final Cut u otro, si su formato es legible) → lista de materiales realmente usados y su ruta.
3. Carpeta de assets. 4. Fuentes. 5. Dossier de investigación. 6. Guion (texto de narración).
7. Música y SFX (+ licencias/recibos). 8. Créditos. 9. Descripción prevista para YouTube.

**Identificación del vídeo auditado.** No asumir que un archivo es "el vídeo final" por su nombre. Listar los candidatos razonables (exportaciones en la carpeta del proyecto y en la carpeta de exportación del editor) y comparar lo disponible: duración frente a la del proyecto, resolución, fecha de modificación frente a la última edición del proyecto, nombre, y contenido (p. ej. créditos o rótulos en fotogramas). Indicar el archivo elegido y en qué se basa. Si no se puede determinar con confianza suficiente ⇒ [NO VERIFICADO], explicar por qué y tratarlo como vídeo final no localizado. Nunca inventar la identificación.

Lo que falta se reporta con su estado (§2); **un elemento no localizable no se da por correcto**. Si falta el vídeo final o el guion, la decisión no puede ser 🟢.
No puedo ver ni oír el vídeo como un espectador: lo que dependa de percepción (p. ej. si una recreación "parece archivo") marcarlo como [NO VERIFICADO] o comprobarlo con fotogramas extraídos (p. ej. con ffmpeg) y decirlo.

### 5.2 Auditar las cuatro áreas
Para cada alerta: **hecho** (qué se observó, archivo/timecode, con etiqueta de §2) · **interpretación** · **riesgo** · **base** (P1–P5, criterio de la skill K1–K4 o fuente del dossier).

**A. Copyright / derechos.** Inventario de CADA asset usado (imagen, vídeo, entrevista, música, SFX, documento, mapa, material de Internet/YouTube, asset IA): origen · autor · licencia · ¿uso comercial? · atribución exigida y presente en créditos/descripción · prueba (URL, recibo, captura). Aplicar §3.
Licencia desconocida o sin prueba ⇒ **DERECHOS NO VERIFICADOS** (🟡). 🔴 solo con evidencia (§2): p. ej. material de terceros identificado como protegido (canción comercial, clip de TV/película, foto con titular conocido) sin licencia ni permiso para el uso, o términos leídos que excluyen el uso comercial.

**B. Monetización / originalidad (P1).** Evaluar el resultado final, no la herramienta; usar IA no es un problema por sí mismo. Comprobar: narración y guion propios; investigación/análisis propio visible (contraste de fuentes, conclusiones propias); edición sustancial; proporción de metraje de terceros frente a material propio; si el vídeo solo lee o resume una fuente ajena; si el formato es plantilla repetida respecto a otros vídeos del canal; si un espectador identifica claramente el aporte propio.
🔴 si es mayoritariamente material ajeno sin transformación o parece producción en masa; 🟡 si hay tramos largos dependientes de un solo tercero o aporte poco visible.

**C. IA / transparencia (P3).** Listar cada elemento sintético o alterado con timecode: imágenes/vídeo IA, recreaciones fotorrealistas, voz sintética, alteraciones de material real (colorizado, restauración generativa, upscaling con inpainting), recreaciones de personas reales o hechos históricos. Clasificar:
- **Asistencia creativa** (guion, subtítulos, miniatura, ilustración claramente no realista, retoque de color) ⇒ P3 no exige divulgación.
- **Sintético/alterado significativo y realista** ⇒ P3: marcar la casilla de contenido alterado/sintético en YouTube Studio.
- Voz sintética que no suplanta a nadie ni se presenta como grabación real ⇒ P3 no la exige expresamente (**interpretación**, no regla).
Además, K1 (criterio de la skill): comprobar rótulo visible y completo en los fotogramas donde una imagen pueda confundirse con archivo.
Recreación realista presentada expresamente como archivo auténtico ⇒ 🔴. Casilla de P3 ausente o sin confirmar, o K1 incumplido sin indicio de engaño ⇒ 🟡.

**D. Exactitud documental.** Cotejar guion (y rótulos) contra dossier y fuentes:
- cada afirmación factual tiene fuente; fechas, nombres, cifras, citas y declaraciones atribuidas coinciden con la fuente;
- K3: [HECHO] / [HIPÓTESIS] / [CONTROVERSIA] / [NO VERIFICADO] separados en narración y pantalla; ninguna hipótesis narrada como hecho;
- nada que el dossier marque [NO VERIFICADO] aparece como hecho;
- contradicciones entre fuentes se presentan como tales;
- reconstrucciones o dramatizaciones no se presentan como archivo.
Hecho contradicho por fuentes verificadas o [NO VERIFICADO] presentado como hecho en un punto central ⇒ 🔴; errores menores, matices o afirmaciones sin fuente localizada ⇒ 🟡.

**Guion ↔ audio.** Si hay guion y narración, comprobar que coinciden. Usar una transcripción fiable si existe, o una herramienta local de transcripción si está disponible (no es obligatorio instalar nada). Si no hay transcripción ni herramienta ⇒ [NO VERIFICADO]. Nunca dar por hecho que coinciden porque ambos archivos existen. Si hay discrepancias verificables, citarlas con timecode y separar las diferencias de redacción menores de las que cambian un dato (cifra, fecha, nombre, atribución, hecho frente a hipótesis); solo estas últimas cuentan para la exactitud.

**Sin dossier.** El dossier es una ayuda de trazabilidad, no una condición de publicación. Si no existe: indicarlo (la trazabilidad de fuentes es más difícil), cotejar las afirmaciones directamente con las fuentes disponibles y marcar [NO VERIFICADO] lo que no se pueda rastrear. La ausencia de dossier no significa que el vídeo sea incorrecto ni sube por sí sola el color a 🔴.

### 5.3 Decisión
Cada área recibe 🟢/🟡/🔴. Decisión global = el peor color de las cuatro áreas:
- 🔴 **NO PUBLICAR**: al menos un problema crítico (derechos, exactitud, reutilizado, IA/transparencia o cumplimiento) que hace imprudente publicar esta versión.
- 🟡 **CORREGIR ANTES DE PUBLICAR**: problemas que deben resolverse antes de subir.
- 🟢 **APTO PARA PUBLICAR**: no se detectaron problemas críticos con la información disponible. No garantiza monetización ni ausencia de reclamaciones.

### 5.4 Formato final (exacto)

```
════════════════════════════════════
YOUTUBE FINAL AUDITOR
════════════════════════════════════

DECISIÓN:
🟢 / 🟡 / 🔴  <texto de la decisión>

COPYRIGHT:
🟢 / 🟡 / 🔴

MONETIZACIÓN / ORIGINALIDAD:
🟢 / 🟡 / 🔴

IA / TRANSPARENCIA:
🟢 / 🟡 / 🔴

EXACTITUD DOCUMENTAL:
🟢 / 🟡 / 🔴

PROBLEMAS CRÍTICOS:
-

PROBLEMAS QUE DEBEN CORREGIRSE:
-

MATERIAL CON DERECHOS INCIERTOS:
-

MATERIAL DE IA QUE REQUIERE REVISIÓN:
-

AFIRMACIONES QUE REQUIEREN VERIFICACIÓN:
-

CAMBIOS OBLIGATORIOS ANTES DE PUBLICAR:
-

ELEMENTOS QUE ESTÁN CORRECTOS:
-

FUENTES DE POLÍTICA DE YOUTUBE UTILIZADAS:
- <título> — <URL> — consultada <fecha>

FECHA DE LA AUDITORÍA:
- <AAAA-MM-DD>

════════════════════════════════════
```

Reglas del informe: cada viñeta cita archivo/timecode, su etiqueta de evidencia (§2) y la política, criterio de la skill o fuente que la justifica; secciones vacías = "- Ninguno"; añadir una línea "Evidencia faltante:" dentro de PROBLEMAS QUE DEBEN CORREGIRSE con cada material de §5.1 o ruta no comprobada y su estado (§2: inexistente / no encontrado / no accesible / insuficiente); el vídeo auditado y su identificación (§5.1) van como primera viñeta de ELEMENTOS QUE ESTÁN CORRECTOS si es [VERIFICADO], o de PROBLEMAS QUE DEBEN CORREGIRSE si es [NO VERIFICADO]. Tras el informe, no aplicar correcciones: el usuario decide.
