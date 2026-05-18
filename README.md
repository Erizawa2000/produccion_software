## Resumen general

| Repositorio | Vulnerabilidades | Workflows CI/CD | Herramientas aplicadas |
|---|---|---|---|
| app_biblio_gestion | 0 | No relevantes | Syft, CodeQL |
| claude-code-action | 5 | Sí | Syft, Grype, CodeQL |
| genai-code-review | 3 | Sí | Syft, Grype, CodeQL |
| great-joy-taxi | 0 | No relevantes | Syft, CodeQL |
| opencode | 16 | Sí | Syft, Grype, CodeQL |

## Metodología utilizada

El análisis fue desarrollado en cuatro etapas:

1. Generación de SBOMs mediante Syft.
2. Detección de vulnerabilidades utilizando Grype.
3. Análisis estático de código con CodeQL.
4. Revisión manual de workflows CI/CD y configuraciones GitHub Actions.

# Gestión de vulnerabilidades en la cadena de suministro de software

## 1. Contexto del análisis

El presente trabajo tiene como objetivo analizar vulnerabilidades asociadas a la cadena de suministro de software (Software Supply Chain Security) a partir de distintos repositorios públicos de GitHub.

Como parte de las actividades previas del módulo, se realizó:

- generación de SBOMs (Software Bill of Materials),
- identificación de vulnerabilidades en dependencias,
- análisis de código fuente,
- revisión de pipelines CI/CD,
- recopilación de evidencia técnica asociada a los hallazgos encontrados.

El análisis fue realizado sobre cinco repositorios públicos utilizando herramientas automatizadas de análisis de dependencias y seguridad, con el objetivo de construir una propuesta de gestión de vulnerabilidades basada en evidencia.

Las herramientas utilizadas durante el análisis fueron:

- Syft: generación de SBOMs.
- Grype: detección de vulnerabilidades en dependencias.
- CodeQL: análisis estático de código fuente.
- GitHub Actions review: análisis de workflows CI/CD y automatizaciones.

Los resultados obtenidos permitieron identificar riesgos asociados a:

- dependencias vulnerables,
- configuraciones inseguras en pipelines CI/CD,
- prácticas de automatización con permisos elevados,
- y decisiones operacionales que pueden incrementar la superficie de ataque de los sistemas analizados.

La propuesta presentada en este documento sigue el ciclo:

Conozco → Verifico → Evidencio → Decido y Actúo

con el fin de establecer una gestión de vulnerabilidades orientada a priorización, mitigación y reducción del riesgo.

## 2. Repositorios analizados

Los siguientes repositorios fueron seleccionados para realizar el análisis de seguridad de la cadena de suministro:

| Repositorio | Descripción general | Tecnologías principales |
|---|---|---|
| app_biblio_gestion | Aplicación de gestión bibliotecaria | Python |
| claude-code-action | GitHub Action orientada a automatización con Claude | Node.js / GitHub Actions |
| genai-code-review | Herramienta de revisión automática de código con IA | Python |
| great-joy-taxi | Proyecto de aplicación web con múltiples dependencias | JavaScript / Python |
| opencode | Plataforma de automatización y tooling moderno | Rust / Node.js / GitHub Actions |

Durante el análisis se generaron SBOMs, reportes de vulnerabilidades y resultados de análisis estático para cada repositorio.

## 3. Vulnerabilidades encontradas

El análisis realizado mediante Grype y CodeQL permitió identificar múltiples vulnerabilidades y riesgos asociados tanto a dependencias de terceros como a configuraciones de automatización CI/CD.

Las vulnerabilidades identificadas corresponden principalmente a:

- vulnerabilidades en librerías y paquetes,
- riesgos asociados a manejo HTTP,
- problemas relacionados con validación TLS,
- riesgos de escritura arbitraria de archivos,
- configuraciones inseguras en workflows CI/CD,
- y automatizaciones con permisos elevados.

### Resumen de vulnerabilidades identificadas

| Repositorio | Vulnerabilidades detectadas | Severidad predominante |
|---|---|---|
| app_biblio_gestion | 0 | No aplicable |
| claude-code-action | 5 | Low |
| genai-code-review | 3 | Low |
| great-joy-taxi | 0 | No aplicable |
| opencode | 16 | Low |

### Vulnerabilidades destacadas

| Repositorio | Vulnerabilidad | Componente afectado | Tipo |
|---|---|---|---|
| claude-code-action | HTTP Request/Response Smuggling | undici | Dependencia |
| claude-code-action | CRLF Injection | undici | Dependencia |
| genai-code-review | Credentials Leak vía `.netrc` | requests | Dependencia |
| opencode | Arbitrary File Write | actions/download-artifact@v4 | CI/CD |
| opencode | Origin Confusion | tauri | Dependencia |
| opencode | Integer Overflow | bytes | Dependencia |
| opencode | Symlink Abuse | tar-rs | Dependencia |

## 4. Clasificación según vector de ataque

Las vulnerabilidades y riesgos identificados fueron clasificados considerando los tres vectores de ataque estudiados en el módulo.

### 4.1 Dependencias y código fuente

Se identificaron vulnerabilidades asociadas a paquetes y librerías utilizadas por los proyectos analizados.

Entre los principales hallazgos destacan:

- vulnerabilidades HTTP en `undici`,
- filtración potencial de credenciales en `requests`,
- problemas de validación TLS en `rustls-webpki`,
- vulnerabilidades de escritura arbitraria y manejo inseguro de archivos en `tar-rs`,
- riesgos de denegación de servicio e integer overflow.

Estas vulnerabilidades afectan componentes utilizados directamente por las aplicaciones y podrían ser explotadas dependiendo del contexto operativo y exposición del sistema.

### 4.2 Pipelines CI/CD

El análisis de workflows GitHub Actions permitió identificar múltiples riesgos relacionados con automatización y pipelines de integración continua.

Entre los principales hallazgos se encontraron:

- uso de `pull_request_target` en workflows de `opencode`,
- uso de permisos elevados como:
  - `contents: write`,
  - `pull-requests: write`,
  - `issues: write`,
  - `id-token: write`,
- uso de actions externas versionadas mediante:
  - `@main`,
  - `@latest`,
- presencia de dependencias vulnerables asociadas a GitHub Actions.

También se identificó el uso parcial de commit hash pinning en algunas actions, lo que representa una práctica más segura frente a ataques de supply chain.

### 4.3 Humanos

Se identificaron riesgos relacionados con decisiones operacionales y prácticas de mantenimiento de seguridad.

Entre ellos destacan:

- uso inconsistente de version pinning en workflows,
- dependencia de automatizaciones externas,
- necesidad de revisión periódica de dependencias,
- gestión de permisos elevados en workflows,
- y necesidad de validación continua de alertas de seguridad.

Estos factores pueden aumentar la superficie de ataque incluso cuando las vulnerabilidades técnicas poseen severidad baja.

## 5. Análisis usando el ciclo Conozco → Verifico → Evidencio → Decido y Actúo

### 5.1 Dependencias vulnerables

#### Conozco

El análisis realizado mediante Grype permitió identificar vulnerabilidades presentes en dependencias utilizadas por distintos repositorios analizados.

Entre los principales hallazgos destacan:

- vulnerabilidades HTTP y de manejo de conexiones en `undici`,
- problemas de filtración de credenciales y verificación TLS en `requests`,
- vulnerabilidades relacionadas con escritura arbitraria de archivos en `actions/download-artifact`,
- y problemas de validación y parsing en librerías como `rustls-webpki`, `tar-rs` y `bytes`.

Estas dependencias forman parte directa del funcionamiento de los sistemas analizados y podrían impactar la seguridad del software dependiendo de su contexto de ejecución.

---

#### Verifico

Las vulnerabilidades fueron verificadas utilizando:

- SBOMs generados con Syft,
- análisis de dependencias mediante Grype,
- validación de versiones instaladas,
- revisión de paquetes afectados,
- y análisis de los componentes efectivamente utilizados por cada proyecto.

Además, se revisó si las vulnerabilidades correspondían a dependencias reales del sistema y no únicamente a paquetes transitorios sin uso aparente.

---

#### Evidencio

La evidencia utilizada para respaldar el análisis incluye:

- archivos SBOM en formato JSON,
- reportes normalizados de Grype,
- reportes RAW de vulnerabilidades,
- archivos `package.json`,
- `requirements.txt`,
- `Cargo.toml`,
- y archivos lock asociados a dependencias.

Entre los hallazgos más relevantes se encuentran:

| Vulnerabilidad | Componente |
|---|---|
| HTTP Request Smuggling | undici |
| CRLF Injection | undici |
| Credential Leak | requests |
| Arbitrary File Write | actions/download-artifact |
| Integer Overflow | bytes |

---

#### Decido y Actúo

Se propone:

- actualizar dependencias vulnerables,
- implementar revisión periódica de librerías,
- automatizar análisis SBOM y escaneo de vulnerabilidades,
- incorporar validación automática en CI/CD,
- y establecer políticas de actualización de dependencias críticas.

También se recomienda utilizar version pinning y monitoreo continuo de dependencias externas para reducir el riesgo asociado a ataques de supply chain.

### 5.2 Riesgos asociados a pipelines CI/CD

#### Conozco

El análisis de workflows GitHub Actions permitió identificar configuraciones que podrían aumentar la superficie de ataque de los pipelines CI/CD.

Se observaron:

- múltiples workflows con permisos de escritura,
- uso de `pull_request_target`,
- automatizaciones con acceso a pull requests e issues,
- uso de actions externas versionadas mediante `@main` y `@latest`,
- y dependencia de automatizaciones de terceros.

---

#### Verifico

Los workflows fueron revisados manualmente mediante análisis de los archivos YAML presentes en `.github/workflows/`.

Se identificaron permisos elevados como:

- `contents: write`,
- `pull-requests: write`,
- `issues: write`,
- `id-token: write`.

Además, se verificó el uso de actions externas y la existencia parcial de commit hash pinning.

---

#### Evidencio

La evidencia utilizada incluye:

- workflows GitHub Actions analizados,
- configuraciones YAML,
- resultados de búsqueda automatizada mediante comandos `grep`,
- y reportes de dependencias asociadas a GitHub Actions.

Entre los hallazgos más relevantes destacan:

| Hallazgo | Riesgo asociado |
|---|---|
| Uso de `pull_request_target` | Mayor riesgo ante PRs maliciosos |
| Uso de `@main` y `@latest` | Cambios no controlados |
| Permisos de escritura | Mayor impacto ante compromiso |
| actions/download-artifact vulnerable | Riesgo de escritura arbitraria |

---

#### Decido y Actúo

Se propone:

- aplicar commit hash pinning en actions externas,
- minimizar permisos en workflows,
- evitar uso innecesario de `pull_request_target`,
- revisar periódicamente workflows automatizados,
- y establecer controles de revisión para cambios en pipelines CI/CD.

También se recomienda aplicar el principio de mínimo privilegio y limitar el uso de automatizaciones con acceso de escritura.

### 5.3 Riesgos asociados a factores humanos

#### Conozco

Durante el análisis se identificaron prácticas operacionales que pueden incrementar el riesgo de seguridad aun cuando las vulnerabilidades técnicas presenten severidad baja.

Entre ellas destacan:

- uso inconsistente de version pinning,
- dependencia de automatizaciones externas,
- permisos elevados en workflows,
- y necesidad de monitoreo continuo de dependencias.

---

#### Verifico

La verificación se realizó mediante revisión manual de configuraciones CI/CD, dependencias utilizadas y prácticas observadas en los repositorios analizados.

Se observó que algunas actions utilizan commit hash pinning mientras otras continúan utilizando referencias dinámicas como `@main` o `@latest`.

---

#### Evidencio

La evidencia utilizada incluye:

- workflows GitHub Actions,
- configuraciones YAML,
- referencias de versionado observadas,
- y dependencias detectadas en los SBOMs.

---

#### Decido y Actúo

Se propone:

- establecer políticas de versionado seguro,
- definir revisiones periódicas de seguridad,
- capacitar al equipo en riesgos de supply chain,
- y formalizar procedimientos de revisión de workflows y dependencias.

Además, se recomienda incorporar controles automáticos de validación de seguridad dentro del ciclo de desarrollo.

## 6. Priorización de vulnerabilidades

La priorización de vulnerabilidades se realizó considerando:

- severidad reportada,
- impacto potencial,
- exposición del componente,
- facilidad de explotación,
- evidencia disponible,
- y alcance dentro de los pipelines y sistemas analizados.

Aunque la mayoría de las vulnerabilidades identificadas fueron clasificadas como severidad “Low”, algunas fueron consideradas de mayor prioridad debido al contexto operativo y al impacto potencial asociado a automatizaciones CI/CD o manejo de credenciales.

### Criterios utilizados

| Criterio | Descripción |
|---|---|
| Severidad | Nivel de severidad reportado por la herramienta |
| Explotabilidad | Facilidad de explotación |
| Impacto | Posible impacto sobre el sistema |
| Exposición | Nivel de acceso o superficie afectada |
| Evidencia | Confirmación mediante análisis |
| Mitigación | Disponibilidad de solución o parche |

### Vulnerabilidades priorizadas

| Vulnerabilidad | Prioridad | Justificación |
|---|---|---|
| Arbitrary File Write (`actions/download-artifact`) | Alta | Impacta pipelines CI/CD y automatización |
| HTTP Request Smuggling (`undici`) | Alta | Riesgo asociado a comunicaciones HTTP |
| Credential Leak (`requests`) | Media-Alta | Posible exposición de credenciales |
| Uso de `@main` y `@latest` | Media-Alta | Riesgo supply chain por cambios no controlados |
| Permisos elevados en workflows | Media | Mayor impacto ante compromiso del pipeline |
| Integer Overflow (`bytes`) | Media | Riesgo técnico asociado a manejo de memoria |

La priorización considera no solo la severidad reportada, sino también el contexto operativo y la superficie de ataque asociada.

## 7. Acciones propuestas

A partir de los hallazgos identificados, se proponen las siguientes acciones para gestionar las vulnerabilidades y reducir el riesgo asociado a la cadena de suministro de software.

### Gestión de dependencias

- actualizar dependencias vulnerables,
- incorporar monitoreo continuo de vulnerabilidades,
- automatizar generación de SBOMs,
- integrar escaneo automático mediante Grype o herramientas similares,
- establecer políticas de actualización periódica.

### Seguridad CI/CD

- aplicar commit hash pinning en GitHub Actions,
- minimizar permisos en workflows,
- evitar uso innecesario de `pull_request_target`,
- revisar workflows automatizados periódicamente,
- limitar automatizaciones con permisos de escritura.

### Gestión organizacional y humana

- capacitar al equipo en riesgos de supply chain,
- definir procedimientos de revisión de dependencias,
- formalizar revisión de workflows y automatizaciones,
- incorporar validaciones de seguridad en pull requests,
- y establecer políticas de seguridad para automatizaciones externas.

### Automatización y monitoreo

- integrar análisis de seguridad dentro del pipeline CI/CD,
- habilitar alertas automáticas de dependencias vulnerables,
- utilizar herramientas de análisis estático de manera continua,
- y mantener trazabilidad de vulnerabilidades identificadas.

## 8. Evidencia utilizada

La propuesta presentada se encuentra respaldada por evidencia técnica obtenida durante el análisis.

### Archivos utilizados

| Evidencia | Ubicación |
|---|---|
| SBOMs generados con Syft | `/data/results/*-sbom.json` |
| Reportes Grype normalizados | `/data/results/*-grype.json` |
| Reportes RAW Grype | `/data/results/*-grype-raw.json` |
| Resultados CodeQL | `/data/results/*-codeql/` |
| Reportes SARIF | `/data/results/*.sarif` |
| Workflows GitHub Actions | `/data/repos/*/.github/workflows/` |

### Evidencia adicional

El análisis también consideró:

- configuraciones YAML,
- archivos `package.json`,
- `requirements.txt`,
- `Cargo.toml`,
- y comandos de análisis ejecutados dentro del entorno de desarrollo.

Toda la evidencia se encuentra disponible dentro del repositorio para permitir la trazabilidad y reproducibilidad del análisis realizado.

## 9. Conclusiones

El análisis realizado demuestra que la seguridad de la cadena de suministro de software no depende únicamente de vulnerabilidades críticas, sino también de configuraciones operacionales, automatizaciones y prácticas de mantenimiento que pueden incrementar la superficie de ataque de un sistema.

A través de la generación de SBOMs, el análisis de dependencias, la revisión de workflows CI/CD y el análisis estático de código, fue posible identificar riesgos asociados a:

- dependencias vulnerables,
- automatizaciones con permisos elevados,
- uso inseguro de GitHub Actions,
- y prácticas inconsistentes de versionado y mantenimiento.

La aplicación del ciclo:

Conozco → Verifico → Evidencio → Decido y Actúo

permitió construir una propuesta de gestión basada en evidencia técnica y priorización contextual del riesgo.

Las acciones propuestas buscan fortalecer la seguridad de los sistemas analizados mediante:

- monitoreo continuo,
- automatización de análisis,
- reducción de privilegios,
- mejores prácticas CI/CD,
- y control de dependencias externas.

Finalmente, el trabajo evidencia la importancia de abordar la seguridad de la cadena de suministro de software desde una perspectiva integral que considere simultáneamente factores técnicos, operacionales y humanos.
