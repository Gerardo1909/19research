# Principios de Prompting (Lecciones MARS condensadas)

Heurísticas extraídas del artículo *Multi-Agent Research System* de Anthropic, adaptadas a este sistema. **Lectura obligatoria** para todos los agentes.

---

## 1. Memoria externa antes de delegar

**Regla:** el Lead Agent escribe su plan completo a `memory/research-state.json` (y a Notion) **antes** de invocar a cualquier subagente.

**Por qué:** el contexto se trunca a partir de 200K tokens; sin persistencia externa, el plan se pierde en investigaciones largas.

## 2. Effort scaling explícito

Calibrar la profundidad al tipo de consulta:

| Tipo de query | Subagentes | Tool calls c/u |
|---|---|---|
| Dato puntual / definición | 1 | 3–5 |
| Revisión de literatura | 3–5 (en paralelo) | 5–10 |
| Investigación completa multi-eje | 5+ | 10+ |

**Regla:** nunca lanzar 50 subagentes para una pregunta trivial (MARS reporta esto como modo de falla frecuente).

## 3. Delegación detallada (briefing mandatorio)

Todo subagente recibe explícitamente:

```xml
<briefing>
  <objetivo>Qué pregunta concreta debe responder</objetivo>
  <output_esperado>Schema/formato exacto del retorno</output_esperado>
  <fuentes_recomendadas>Bases, autores, períodos sugeridos</fuentes_recomendadas>
  <criterio_de_exito>Cuándo se considera completa la tarea</criterio_de_exito>
  <limites>Lo que NO debe hacer (evitar duplicar trabajo de otros)</limites>
</briefing>
```

Sin briefing detallado → los subagentes duplican búsquedas o se desvían (lección #1 de MARS).

## 4. Búsqueda breadth-first

**Regla:** la primera tool call siempre con keywords amplias. Refinar en calls 2..N en función de lo encontrado.

**Por qué:** los modelos tienden a queries hiper-específicas que no devuelven nada útil.

## 5. Paralelismo de tool calls

Cuando un subagente tiene **keywords independientes**, lanza ≥ 3 búsquedas en paralelo (no secuencial).

**Impacto:** MARS reporta –90% de latencia en queries complejas.

## 6. Heurística anti-SEO

Priorizar journals indexados sobre contenido optimizado para Google. Refinar con:

- `site:scholar.google.com`, `site:doi.org`, `site:jstor.org`
- `filetype:pdf` para papers
- Nombres explícitos de bases: Scopus, JSTOR, SciELO, RedALyC, SSRN, NBER

## 7. Extended thinking como scratchpad

Cuando el agente lo soporte, usar `<thinking>` para:
- Planificar la estrategia antes de la primera tool call.
- Evaluar la calidad de cada resultado antes de seguir.
- Decidir si continuar el loop o salir.

## 8. Loop de auto-mejora

Cada vez que el Peer Reviewer emite veredicto ≠ `accept`, registrar en `guidelines/aprendizajes.xml`:

```xml
<leccion fecha="YYYY-MM-DD">
  <patron_de_error>Descripción del fallo recurrente</patron_de_error>
  <agente_responsable>nombre_del_agente</agente_responsable>
  <mitigacion>Regla concreta para evitarlo en el futuro</mitigacion>
</leccion>
```

El próximo ciclo, el agente afectado debe leer `aprendizajes.xml` antes de actuar.

## 9. Reportar contradicciones, no sólo consensos

Los subagentes deben documentar **explícitamente** los desacuerdos entre fuentes (no esconderlos para mostrar una narrativa limpia).

## 10. Verificá, no asumas

Toda afirmación factual debe ser trazable a una fuente del fichero bibliográfico. Si no hay fuente, marcar con `<citation_missing/>` y derivar al Lead para nueva búsqueda.
