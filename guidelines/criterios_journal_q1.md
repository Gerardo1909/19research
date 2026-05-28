# Criterios de Calidad — Estándar Journal Q1

Rúbrica que **todos los agentes** usan para juzgar la calidad de fuentes y artefactos. Inspirada en los estándares de revistas científicas indexadas en Scopus/JCR cuartil 1.

---

## A. Jerarquía de fuentes (de mayor a menor confiabilidad)

1. **Revisiones de literatura sistemáticas** en journals Q1 (mejor punto de partida).
2. **Artículos empíricos** en journals Q1/Q2 indexados (Scopus, JCR, SciELO con peer review).
3. **Capítulos de libros** en editoriales académicas reconocidas (Cambridge, Oxford, Springer, Routledge, FCE, Siglo XXI, etc.).
4. **Working papers** institucionales (NBER, CEPAL, BID, CONICET, World Bank) — válidos pero etiquetar como "no peer-reviewed".
5. **Tesis doctorales** defendidas en programas acreditados.
6. **Informes técnicos** de organismos oficiales (INDEC, BCRA, OECD, UN, etc.) — para datos, no para marco teórico.
7. **Actas de congreso** indexadas (sólo si no hay versión journal).

## B. Fuentes a DESCARTAR (salvo que sean objeto de estudio)

- Blogs personales / Medium / LinkedIn articles.
- Notas periodísticas de opinión.
- Content farms SEO-optimizadas (Investopedia, Wikipedia, etc., como fuente primaria).
- Reportes de consultoras sin metodología publicada.
- Posts de redes sociales.

> **Heurística MARS anti-SEO**: si una búsqueda devuelve resultados optimizados para Google antes que journals, refinar la query agregando `site:scholar.google.com`, `filetype:pdf`, o nombres de bases indexadas.

## C. Metadatos obligatorios por fuente

Para que una fuente entre al fichero bibliográfico debe tener:

- `autores`, `año`, `título`, `revista/editorial`, `volumen/número/páginas`.
- `DOI` o URL indexada (Google Scholar, Scopus, repositorio institucional).
- `índice de impacto` o `cuartil` cuando aplique.
- `idioma original`.

## D. Rúbrica de calidad del artefacto final (0–3 por eje)

| Eje | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
| **Brecha (research gap)** | No identificada | Insinuada sin sustento | Identificada con 1–2 fuentes | Identificada y triangulada con ≥3 fuentes y controversia explícita |
| **Estado de la cuestión** | Bloque indiferenciado | Sólo teoría o sólo empiria | Ambas dimensiones, desbalanceadas | Balance teórico/empírico con comunidad académica y controversias mapeadas |
| **Pregunta de investigación** | Genérica o ausente | Amplia, no operacionalizable | Acotada pero binaria/superficial | Precisa, acotada, operacionalizable, deriva del gap |
| **Metodología** | Inviable o ausente | Datos no accesibles o técnica inadecuada | Coherente pero con limitaciones no declaradas | Acoplada a la pregunta, datos verificables, limitaciones explícitas |
| **Rigor de citación** | Plagio o asunciones sin fuente | Citas incompletas o mal formateadas | Citas correctas pero sin DOI/URL | Todas las afirmaciones citadas con trazabilidad completa |

**Veredicto editorial:**
- `accept` → todos los ejes en 3.
- `minor_revision` → todos en ≥ 2; correcciones menores.
- `major_revision` → al menos un eje en 1 o 2 estructural.
- `reject` → dos o más ejes en 0/1.
