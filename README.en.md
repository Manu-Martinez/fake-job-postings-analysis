🇺🇸 English | 🇦🇷 [Español](README.md)

# Fake job postings: a problem that grew 7x

Data analysis on fraudulent job postings — from classic employment scams to their use as a vector for sophisticated hacking attacks.

## The core finding

1 in 20 job postings analyzed is fake. And while LinkedIn's detection of fake accounts multiplied by 7 since 2020, the problem evolved: it's no longer just about money scams — it's a real vector for delivering malware, with campaigns documented as recently as 2026.

## What's in this repository

- `data/` — the 7 datasets used in the analysis
- Interactive Power BI dashboard (two pages: Executive Summary and Fraudulent Postings)
- Summary carousel published on LinkedIn

## Datasets and methodology

This project combines three distinct types of sources. It's important to be transparent about the nature of each one.

### 1. Academic dataset (direct download)

**`fake_job_postings.csv`** — EMSCAD dataset (Employment Scam Aegean Dataset), University of the Aegean. 17,880 real job postings (2012-2014), manually annotated as legitimate or fraudulent.
Columns: `job_id`, `title`, `location`, `department`, `salary_range`, `company_profile`, `description`, `requirements`, `benefits`, `telecommuting`, `has_company_logo`, `has_questions`, `employment_type`, `required_experience`, `required_education`, `industry`, `function`, `fraudulent`.

**`fake_job_postings_procesado.csv`** — same base, with `salary_min` and `salary_max` extracted from `salary_range`, plus `type_of_job_posting` added. 29 records (1%) excluded due to extreme salary outliers.

### 2. Official third-party data (compiled from public reports)

**`linkedin_cuentas_falsas.csv`** — fake accounts detected per half-year, according to LinkedIn's official transparency reports (LinkedIn Community Report).
Columns: `semestre`, `anio`, `periodo_label`, `cuentas_falsas_registro_millones`, `pct_detenidas_proactivamente`, `fuente`.

### 3. Original compilation, curated from security sources (not a downloaded dataset)

**`campanias_hacking_documentadas.csv`** — **manually built by the author**, not downloaded from any repository. Each row documents a real hacking campaign delivered via fake job postings, reported by recognized cybersecurity firms (Microsoft Security Blog, ESET Research, ReversingLabs, SentinelOne, Silent Push, IBM X-Force, NVISO Labs, Picus Security, DomainTools).
Columns: `fecha_aprox`, `anio`, `campania`, `actor_atribuido`, `tecnica_entrega`, `malware_o_backdoor`, `sector_objetivo`, `fuente`.

**Important:** these are 10 documented cases, not a statistically representative sample. No statistical significance testing is applied — they're presented as concrete, verifiable cases, each with its source.

**Note on gaps:** there is no documented case from 2023. This doesn't imply an absence of campaigns that year — it only reflects the limits of this particular compilation.

### Files derived from the analysis

- **`palabras_distintivas_fraude.csv`** — words overrepresented in fraudulent vs. legitimate descriptions. Columns: `palabra`, `frecuencia_fraude`, `frecuencia_legitima`, `ratio_sobrerrepresentacion`. Filtered against an English dictionary (`wordfreq` library) to exclude corrupted/concatenated text artifacts from the original dataset.
- **`salario_boxplot_us.csv`** — salary distribution filtered to US postings only (to avoid mixing currencies without a reliable conversion) and without extreme outliers. Columns: `job_id`, `fraudulent`, `fraudulent_label`, `salary_min`, `salary_max`.
- **`salario_informado_por_clase.csv`** — proportion of postings that disclose salary, by class. Columns: `clase`, `pct_informa_salario`, `cantidad_total`, `cantidad_con_salario`.

### Columns and measures calculated directly in Power BI (not in the CSVs)

To keep the original datasets untouched, several columns and measures were added inside the Power BI model instead of in the source files:

- `fake_job_postings_procesado`: measure `Porcentaje Ofertas Fraudulentas`
- `linkedin_cuentas_falsas`: measures `Cuentas 2020`, `Cuentas Ultimo Periodo`, `Crecimiento LinkedIn`, `Crecimiento LinkedIn Texto`
- `campanias_hacking_documentadas`: calculated columns `actor_agrupado` and `sector_agrupado` (group equivalent text variants, e.g. different mentions of Lazarus Group under a single category), `Fecha_evento` column (a real date built from `anio`, used for chronological sorting), measure `Total Campañas Documentadas`
- `fake_job_postings`: measure `Total de Ofertas`

## Key findings

1. **5% fraud rate** in the EMSCAD dataset (866 of 17,880 postings)
2. **Fraudulent postings disclose salary 67% more often** than legitimate ones (25.8% vs. 15.5%) — the salary median itself is actually *lower* in fraudulent postings, so the pattern is about frequency, not about promising a bigger number
3. **Energy-sector impersonation pattern**: words like "aker," "subsea," "expro," "aecom" appear up to 140 times more often in fraudulent postings, suggesting a specific, repeated campaign
4. **LinkedIn's detection of fake accounts grew 7x** between 2020 (11.6M per half-year) and 2025 (83.4M)
5. **9 of the 10 documented hacking campaigns are attributed to Lazarus Group** (North Korea) — a single actor dominates the landscape
6. **Techniques evolved significantly**: from trojanized PDFs (2020) to SVG steganography and malicious git hooks (2026)

## Known limitations

- EMSCAD is from 2012-2014; it doesn't necessarily represent current employment scam tactics, although the structural patterns (salary, wording) remain relevant
- The dataset has no per-record date column, only the general period declared by its authors
- The hacking campaigns table is a curated qualitative compilation, not a downloaded dataset or a statistical sample
- Salary data excludes postings outside the US to avoid mixing currencies without a reliable conversion

## How to protect yourself (practical summary)

- If they ask you to pay anything upfront (registration, equipment, training), it's a scam
- If you're offered a position without having applied, be suspicious
- If you can't independently verify the company, don't share your data
- If they ask you to clone and run unknown code, test it in isolation — never on your main machine
- If the recruiter pushes the conversation to WhatsApp or Telegram, be wary

---

**Author:** [Your name] — Data Analyst in training, looking for a remote role.
