# Test Queries — Harness de Evaluación del Sistema

20 queries representativas para validar el sistema multiagente. Cubren distintos niveles de complejidad y modos de falla conocidos en MARS.

## Categoría A — Queries simples (1 subagente, baseline)

1. **Definición conceptual**: ¿Qué se entiende por "capacidades dinámicas" en la literatura de management estratégico desde Teece (1997)?
2. **Dato puntual**: ¿Cuál fue la tasa de inversión productiva sobre PIB en Argentina, año 2010, según el INDEC?
3. **Autor único**: Resumir la tesis central de Acemoglu & Robinson (2012) sobre instituciones y desarrollo.
4. **Definir un debate**: ¿Qué diferencia hay entre el enfoque "varieties of capitalism" de Hall & Soskice y la corriente regulacionista?

## Categoría B — Revisiones de literatura acotadas (3–5 subagentes)

5. Estado del arte sobre **brecha de productividad PYMES vs grandes empresas** en América Latina, 2000-2020.
6. Estado del arte sobre **género y carreras STEM** en universidades públicas argentinas.
7. Estado del arte sobre **uso de LLMs en investigación cualitativa** en ciencias sociales (2022-2025).
8. Estado del arte sobre **políticas de innovación** en países semi-industrializados.

## Categoría C — Investigación completa multi-eje (5+ subagentes)

9. **Pregunta causal**: ¿Las políticas de promoción industrial argentinas (2003-2015) tuvieron impacto en las capacidades tecnológicas de las firmas, controlando por sesgo de selección?
10. **Pregunta mixta**: ¿Cómo se relaciona la heterogeneidad estructural (CEPAL) con la persistencia de la informalidad laboral en Argentina, y qué evidencia empírica reciente apoya o contradice la tesis?
11. **Pregunta exploratoria**: ¿Qué dinámicas explican la emergencia de polos tecnológicos en ciudades intermedias de América Latina?

## Categoría D — Modos de falla deliberados (smoke tests)

12. **Tema con mucho SEO**: "Mejores prácticas de productividad personal" → debería rechazar fuentes self-help y exigir literatura organizacional.
13. **Tema controversial**: "Eficacia de la educación montessori" → debería reportar ambos lados del debate.
14. **Pregunta mal acoplada al método**: "¿Por qué la gente vota como vota?" → el methodologist debería rechazar técnicas cuantitativas observacionales sin estrategia causal.
15. **Datos inexistentes**: "Estimación de mercado negro de dólares en Argentina por provincia, 1980-1990" → debería declarar inviabilidad metodológica.
16. **Fuente única**: pedir investigación sobre un tema donde sólo hay un autor relevante → search_subagent debería reportar limitación.

## Categoría E — Edge cases

17. **Tema interdisciplinario**: ¿Qué aportes hace la economía conductual al diseño de políticas sanitarias en países en desarrollo?
18. **Tema reciente sin literatura asentada**: ¿Impacto laboral de los LLMs en profesiones del conocimiento (2024-2025)?
19. **Tema con controversias metodológicas**: ¿Las RCTs son el gold standard para evaluación de programas sociales? (debate Banerjee/Duflo vs Deaton).
20. **Tema con datos restringidos**: ¿Determinantes del rendimiento académico en universidades argentinas?

---

## Cómo usar este harness

1. Por cada query, correr el flujo completo (N1 → N6) y guardar artefactos en `tasks/eval-{N}-...`.
2. Aplicar `eval/judge_rubric.xml` al manuscrito final.
3. Registrar scores en una tabla. Métrica de éxito global: **≥ 80% de queries con manuscrito en veredicto `minor_revision` o mejor en ≤ 3 iteraciones**.
4. Las queries de categoría D deben **fallar de forma controlada** — el sistema debe detectar el problema, no producir un manuscrito espurio.
