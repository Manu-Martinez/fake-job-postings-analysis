🇦🇷 Español | 🇺🇸 [English](README.en.md)

# Ofertas de empleo falsas: un problema que creció x7

Análisis de datos sobre ofertas de empleo fraudulentas — desde la estafa laboral clásica hasta su uso como vector de ataques de hacking sofisticados.

## El hallazgo central

1 de cada 20 ofertas de empleo analizadas es falsa. Y mientras LinkedIn multiplicó por 7 la detección de cuentas falsas desde 2020, el problema evolucionó: ya no es solo estafa de dinero, es un vector real para instalar malware, con campañas documentadas activas hasta 2026.

## Dashboard

### Resumen Ejecutivo
![Resumen Ejecutivo](fake-job-postings-analysis/images/dashboard_resumen_ejecutivo.png)

### Radiografía del Fraude
![Radiografía del Fraude](fake-job-postings-analysis/images/dashboard_radiografia_fraude.png)


## Qué incluye este repositorio

- `data/` — los 7 datasets utilizados en el análisis
- Dashboard interactivo en Power BI (dos hojas: Resumen Ejecutivo y Ofertas Fraudulentas)
- Carrusel resumen publicado en LinkedIn

## Datasets y metodología

Este proyecto combina tres tipos de fuentes distintas. Es importante ser transparente sobre la naturaleza de cada una.

### 1. Dataset académico (descarga directa)

**`fake_job_postings.csv`** — Dataset EMSCAD (Employment Scam Aegean Dataset), Universidad del Egeo. 17,880 ofertas de empleo reales (2012-2014), anotadas manualmente como legítimas o fraudulentas.
Columnas: `job_id`, `title`, `location`, `department`, `salary_range`, `company_profile`, `description`, `requirements`, `benefits`, `telecommuting`, `has_company_logo`, `has_questions`, `employment_type`, `required_experience`, `required_education`, `industry`, `function`, `fraudulent`.

**`fake_job_postings_procesado.csv`** — misma base, con `salary_min` y `salary_max` extraídos de `salary_range`, y `type_of_job_posting` agregada. 29 registros (1%) excluidos por outliers salariales extremos.

### 2. Datos oficiales de terceros (recopilados de reportes públicos)

**`linkedin_cuentas_falsas.csv`** — cuentas falsas detectadas por semestre, según los reportes de transparencia oficiales de LinkedIn (LinkedIn Community Report).
Columnas: `semestre`, `anio`, `periodo_label`, `cuentas_falsas_registro_millones`, `pct_detenidas_proactivamente`, `fuente`.

### 3. Recopilación propia, curada a partir de fuentes de seguridad (no es un dataset descargado)

**`campanias_hacking_documentadas.csv`** — **construida manualmente por el autor**, no descargada de ningún repositorio. Cada fila documenta una campaña real de hacking vía ofertas de empleo falsas, reportada por firmas de ciberseguridad reconocidas (Microsoft Security Blog, ESET Research, ReversingLabs, SentinelOne, Silent Push, IBM X-Force, NVISO Labs, Picus Security, DomainTools).
Columnas: `fecha_aprox`, `anio`, `campania`, `actor_atribuido`, `tecnica_entrega`, `malware_o_backdoor`, `sector_objetivo`, `fuente`.

### 3. Recopilación propia, curada a partir de fuentes de seguridad (no es un dataset descargado)

**`campanias_hacking_documentadas.csv`** — **construida manualmente por el autor**, no descargada de ningún repositorio. Cada fila documenta una campaña real de hacking vía ofertas de empleo falsas, reportada por firmas de ciberseguridad reconocidas (Microsoft Security Blog, ESET Research, ReversingLabs, SentinelOne, Silent Push, IBM X-Force, NVISO Labs, Picus Security, DomainTools).
Columnas: `fecha_aprox`, `anio`, `campania`, `actor_atribuido`, `tecnica_entrega`, `malware_o_backdoor`, `sector_objetivo`, `fuente`.

**Importante:** son 10 casos documentados, no una muestra estadística representativa. No se les aplica significancia estadística — se presentan como casos concretos y verificables, cada uno con su fuente.

**Nota sobre huecos:** no hay ningún caso documentado de 2023. No implica ausencia de campañas ese año, solo el límite de esta recopilación puntual.

### Archivos derivados del análisis

- **`palabras_distintivas_fraude.csv`** — palabras sobrerrepresentadas en descripciones fraudulentas vs. legítimas. Columnas: `palabra`, `frecuencia_fraude`, `frecuencia_legitima`, `ratio_sobrerrepresentacion`. Filtradas contra un diccionario de inglés (librería `wordfreq`) para excluir texto corrupto/concatenado propio del dataset original.
- **`salario_boxplot_us.csv`** — distribución salarial filtrada solo a ofertas de EE.UU. (para no mezclar monedas sin conversión confiable) y sin outliers extremos. Columnas: `job_id`, `fraudulent`, `fraudulent_label`, `salary_min`, `salary_max`.
- **`salario_informado_por_clase.csv`** — proporción de ofertas que informan salario, por clase. Columnas: `clase`, `pct_informa_salario`, `cantidad_total`, `cantidad_con_salario`.

### Columnas y medidas calculadas directo en Power BI (no están en los CSV)

Para mantener los datasets originales intactos, varias columnas y medidas se agregaron dentro del modelo de Power BI en vez de en los archivos fuente:

- `fake_job_postings_procesado`: medida `Porcentaje Ofertas Fraudulentas`
- `linkedin_cuentas_falsas`: medidas `Cuentas 2020`, `Cuentas Ultimo Periodo`, `Crecimiento LinkedIn`, `Crecimiento LinkedIn Texto`
- `campanias_hacking_documentadas`: columnas calculadas `actor_agrupado` y `sector_agrupado` (agrupan variantes de texto equivalentes, ej. distintas menciones a Lazarus Group bajo una sola categoría), columna `Fecha_evento` (fecha real construida a partir de `anio`, usada para ordenar cronológicamente), medida `Total Campañas Documentadas`
- `fake_job_postings`: medida `Total de Ofertas`

## Diccionario de columnas (español → inglés)

Los archivos CSV mantienen sus nombres de columna originales en español. Para quien prefiera revisarlos en inglés, esta tabla traduce los campos más relevantes:

| Español | English |
|---|---|
| `fraudulent` / `fraudulent_label` | fraudulent / fraudulent_label |
| `clase` | class (`Legítima` = Legitimate, `Fraudulenta` = Fraudulent) |
| `salario_min` / `salario_max` (`salary_min`/`salary_max`) | minimum/maximum salary |
| `pct_informa_salario` | % that discloses salary |
| `cantidad_total` / `cantidad_con_salario` | total count / count with salary disclosed |
| `palabra` | word |
| `frecuencia_fraude` / `frecuencia_legitima` | frequency in fraudulent / legitimate postings |
| `ratio_sobrerrepresentacion` | overrepresentation ratio |
| `semestre` / `anio` / `periodo_label` | half-year period / year / period label |
| `cuentas_falsas_registro_millones` | fake accounts detected at registration (millions) |
| `pct_detenidas_proactivamente` | % stopped proactively |
| `fecha_aprox` | approximate date |
| `campania` | campaign |
| `actor_atribuido` / `actor_agrupado` | attributed actor / grouped actor |
| `tecnica_entrega` | delivery technique |
| `malware_o_backdoor` | malware or backdoor |
| `sector_objetivo` / `sector_agrupado` | target sector / grouped sector |
| `fuente` | source |

## Hallazgos principales

1. **5% de fraude** en el dataset EMSCAD (866 de 17,880 ofertas)
2. **Las ofertas fraudulentas informan el salario un 67% más seguido** que las legítimas (25.8% vs. 15.5%) — la mediana de salario en sí es más *baja* en las fraudulentas, así que el patrón es de frecuencia, no de monto prometido
3. **Patrón de impersonación de empresas de energía**: palabras como "aker", "subsea", "expro", "aecom" aparecen hasta 140 veces más en ofertas fraudulentas, sugiriendo una campaña específica y repetitiva
4. **LinkedIn multiplicó por 7 la detección de cuentas falsas** entre 2020 (11.6M por semestre) y 2025 (83.4M)
5. **9 de las 10 campañas de hacking documentadas están atribuidas a Lazarus Group** (Corea del Norte) — un solo actor domina el panorama
6. **Las técnicas evolucionaron significativamente**: de PDFs troyanizados (2020) a esteganografía en archivos SVG y git hooks maliciosos (2026)

## Fuentes citadas

### Datos de LinkedIn
- LinkedIn Community Report (reportes de transparencia semestrales oficiales) — [about.linkedin.com/transparency/community-report](https://about.linkedin.com/transparency/community-report)
- Cobertura secundaria que cita cifras específicas por semestre: Rest of World, Prospect Magazine, VerityAI

### Campañas de hacking documentadas (una fuente por fila del CSV)
| Campaña | Fuente |
|---|---|
| Operation Dream Job (origen, 2020) | ClearSky Research |
| Dream Job — variante contra investigadores (2021) | Google Threat Analysis Group / Microsoft |
| Dream Job — sector químico (2022) | Symantec |
| Operation Interception (2024) | ESET / SentinelOne |
| Contagious Interview — empresas fachada (2024-2025) | Silent Push / Microsoft |
| ClickFake Interview (2025) | Picus Security |
| FIN6 / Skeleton Spider (2025) | DomainTools Investigations / The Record |
| Contagious Interview — servicios JSON (2025) | NVISO Labs |
| Contagious Interview — git hooks (2026) | IBM X-Force |
| Contagious Interview — esteganografía SVG (2026) | Elastic Security Labs |

Cada fuente corresponde a un reporte público de investigación de la firma mencionada. Este proyecto solo utiliza la información descriptiva de esos reportes (fechas, actores, técnicas) — en ningún momento se accedió a muestras de malware, dominios de comando y control, ni repositorios troyanizados mencionados en dichos reportes.

### Dataset académico
- EMSCAD (Employment Scam Aegean Dataset) — Universidad del Egeo, disponible públicamente en Kaggle

## Limitaciones conocidas

- EMSCAD es de 2012-2014; no representa necesariamente las tácticas de estafa laboral actuales, aunque los patrones estructurales (salario, redacción) siguen siendo relevantes
- El dataset no tiene columna de fecha por registro individual, solo el período general declarado por sus autores
- La tabla de campañas de hacking es una recopilación cualitativa curada, no un dataset descargado ni una muestra estadística
- Los datos salariales excluyen ofertas fuera de EE.UU. para evitar mezclar monedas sin una conversión confiable

## Cómo protegerte (resumen práctico)

- Si te piden pagar algo por adelantado (inscripción, equipo, capacitación), es estafa
- Si te ofrecen el puesto sin que hayas aplicado, sospechá
- Si no podés verificar la empresa de forma independiente, no compartas tus datos
- Si piden clonar y correr código desconocido, probalo aislado — nunca en tu máquina principal
- Si el reclutador empuja la charla a WhatsApp o Telegram, desconfiá

---

**Autor:** [Manuel Martinez] — Data Analyst en formación, buscando rol remoto.
