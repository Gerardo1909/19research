# 19research — Sistema Multiagente de Investigación Académica

Sistema de agentes especializados (5 roles) para asistir investigación académica con rigor metodológico. Inspirado en el **Multi-Agent Research System (MARS)** de Anthropic, adaptado al pipeline científico clásico y operado por **copy-paste de prompts XML** (sin orquestación automática).

#### **Hecho por:** Gerardo Toboso 
#### **mail:** gerardotoboso1909@gmail.com

## Arquitectura

```
Problema → Estado de la Cuestión (Teórico + Empírico) → Pregunta → Metodología → Resultados
```

5 agentes:

| Agente | Rol | Archivo |
|---|---|---|
| Lead Researcher | Planificación, delegación, síntesis | `prompts/lead_researcher.xml` |
| Search Subagent | Búsqueda y fichado bibliográfico | `prompts/search_subagent.xml` |
| Methodologist | Validación pregunta↔datos↔técnica | `prompts/methodologist_agent.xml` |
| Citation Agent | Inyección de citas APA y fact-checking | `prompts/citation_agent.xml` |
| Peer Reviewer | Auditoría final y veredicto editorial | `prompts/peer_reviewer.xml` |

Diagrama del flujo: `pipelines/research_flow.xml` (incluye grafo Mermaid).

## Estructura del repo

```
19research/
├── README.md
├── project_config.xml          # Configurás un proyecto por vez acá
├── guidelines/                 # Conocimiento base (todos los agentes leen)
├── pipelines/                  # Grafo de ejecución
├── prompts/                    # Los 5 agentes XML
├── tasks/                      # Outputs versionados por sesión
├── eval/                       # Harness de evaluación
└── memory/                     # Estado compartido (research-state.json)
```

## Cómo usar el sistema (workflow copy-paste)

### Paso 0 — Configurá el proyecto

Editá `project_config.xml`: tema, hipótesis preliminar, alcance espacial/temporal, unidad de análisis, preferencias.

### Paso 1 — Planificación (Lead Researcher)

1. Abrí una sesión nueva de Claude (web o desktop).
2. Pegá el contenido de `prompts/lead_researcher.xml` + `project_config.xml` + todos los `guidelines/*`.
3. Pedile: *"Generá el plan de investigación inicial siguiendo F0–F2."*
4. Guardá el output en `tasks/YYYY-MM-DD-plan.xml`.
5. Copiá el plan a una página de Notion (memoria persistente).

### Paso 2 — Búsquedas en paralelo (Search Subagents)

Por cada `<briefing>` que el Lead generó:

1. Abrí una **sesión separada** de Claude (cada subagente arranca sin contexto del anterior — esto es deliberado).
2. Pegá `prompts/search_subagent.xml` + el briefing específico + `guidelines/fichero_bibliografico_schema.xml` + `guidelines/criterios_journal_q1.md`.
3. Pedile: *"Ejecutá el briefing siguiendo F1–F5. Usá WebSearch en paralelo."*
4. Guardá output en `tasks/YYYY-MM-DD-search-{topic}.xml`.

> Tip: corré 3-5 subagentes en pestañas/ventanas paralelas. Esa es la paralelización "manual" que reemplaza a la del Lead automático de MARS.

### Paso 3 — Consolidación (Lead Researcher, 2da sesión)

1. Sesión nueva: pegá `lead_researcher.xml` + todos los outputs `tasks/YYYY-MM-DD-search-*.xml`.
2. Pedile: *"Consolidá los hallazgos siguiendo F4–F5. Decidí continue_loop o exit_loop."*
3. Si `continue_loop`: volvé al Paso 2 con nuevos briefings.
4. Si `exit_loop`: guardá `tasks/YYYY-MM-DD-manuscript-v1.xml` y avanzá al Paso 4.

### Paso 4 — Metodología (Methodologist)

1. Sesión nueva: pegá `methodologist_agent.xml` + manuscrito + fichero.
2. Pedile: *"Validá el acoplamiento pregunta↔datos↔técnica."*
3. Si `reject`: volvé al Lead. Si `viable`: guardá `tasks/YYYY-MM-DD-methodology-v1.xml` y avanzá.

### Paso 5 — Citas (Citation Agent)

1. Sesión nueva: pegá `citation_agent.xml` + manuscrito + fichero.
2. Pedile: *"Inyectá citas APA-7 y marcá huérfanas con &lt;citation_missing/&gt;."*
3. Guardá `tasks/YYYY-MM-DD-manuscript-cited-v1.xml`.

### Paso 6 — Review (Peer Reviewer)

1. Sesión nueva: pegá `peer_reviewer.xml` + manuscrito citado + `guidelines/criterios_journal_q1.md`.
2. Pedile: *"Auditá según F1–F7. Emití veredicto editorial."*
3. Guardá `tasks/YYYY-MM-DD-review-v1.xml`.
4. **Si veredicto ≠ accept**: copiá la `<leccion>` propuesta a `guidelines/aprendizajes.xml`.

### Paso 7 — Loop

- `accept` → terminaste.
- `minor_revision` → volvé al agente responsable (campo `target_agent` de cada issue).
- `major_revision` → volvé al Lead.
- `reject` → reformulación desde el plan.

Tope: 5 iteraciones. Si no convergís, replanteá el proyecto.

## Evaluación del sistema

Para validar que el sistema funciona:

1. Corré las 20 queries de `eval/test_queries.md` (al menos las de categorías A y D).
2. Aplicá `eval/judge_rubric.xml` a cada manuscrito final.
3. Métrica de éxito: ≥ 80% con `score_global >= 0.7` Y `factual_accuracy >= 0.7` Y `citation_accuracy >= 0.7`.

## Memoria

- `memory/research-state.json`: snapshot local del estado del proyecto (commiteá a git por iteración).
- Notion (opcional): página espejo del JSON; el MCP de Notion ya está disponible si lo querés usar.
- `guidelines/aprendizajes.xml`: lecciones acumuladas — el Peer Reviewer las agrega; todos los agentes las leen.

## Referencias

- Anthropic. *Multi-agent research system*. https://www.anthropic.com/engineering/multi-agent-research-system
- `guidelines/prompting_principles.md` (heurísticas MARS condensadas)
- `guidelines/metodologia_investigacion.md` (pipeline epistemológico)
- `guidelines/criterios_journal_q1.md` (rúbrica de calidad)
