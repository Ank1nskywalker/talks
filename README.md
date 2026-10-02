# Charlas y Conferencias

Colección del material de mis charlas y conferencias sobre ciberseguridad: Threat Intelligence, Threat Hunting, Incident Response y detección de amenazas a nivel de red.

Cada charla incluye un breve resumen y el enlace a las diapositivas, organizadas por año en este mismo repositorio. Las plantillas reutilizables, como el informe de threat hunting, están en la carpeta [Templates](Templates).

---

## Sobre mí

Analista en ciberseguridad con foco en Threat Intelligence, Incident Response y Threat Hunting. Trabajo con organizaciones de Uruguay, Argentina y Chile, tanto del sector público como del financiero.

- **X:** [@anakinswal](https://x.com/anakinswal)

---

## 2026

### Threat Intelligence 101 - From Reactive to Predictive Approach

**Ekoparty Miami** · *May 2026*

A tour of CTI with use cases and reference frameworks (Diamond Model, MITRE ATT&CK, Cyber Kill Chain), source assessment and the Pyramid of Pain, along with an overview of current threats. Includes a hands-on workshop on mapping online adversary infrastructure.

[Slides](2026/Ekoparty%20Miami%20-%20Threat%20Intelligence%20101.pptx) · [PDF](2026/Ekoparty%20Miami%20-%20Threat%20Intelligence%20101%20%28EN%29.pdf)

### Come to Dark Side - We have cookies

**WomenCISO** · *Febrero 2026*

Un recorrido por los orígenes de la Dark Web, sus distintos usos, protocolos y plataformas de acceso, con buenas prácticas para navegarla y la perspectiva de su uso desde las fuerzas de seguridad y los analistas de ciberseguridad.

[Slides](2026/WomenCISO%20-%20Come%20to%20Dark%20Side%20-%20We%20have%20cookies.pptx)

### CTI proactiva para la ciberdefensa: monitoreo de la Dark Web, análisis de TTPs y atribución de actores de amenaza

**CYBER.AR — I Congreso de Ciberdefensa Argentina 2026** · Buenos Aires, Argentina · *Septiembre 2026*

Metodología de CTI proactiva estructurada en tres fases encadenadas: recolección y evaluación de fuentes en la Dark Web con código Admiralty, OPSEC y requerimientos de inteligencia prioritarios; el ascenso del indicador al comportamiento mediante la Pirámide del Dolor y MITRE ATT&CK; y la atribución estructurada en tres niveles —técnico, operacional y político— con Análisis de Hipótesis en Competencia y umbrales de confianza diferenciados. Incluye casos documentados de abuso de herramientas RMM como vector de acceso y de técnicas Living-off-the-Land por actores alineados a Estados. Suma dos casos regionales propios reportados al CERT.ar, la exposición de credenciales de organismos argentinos en mercados de infostealers y el impacto local del caso Oldelval.

[Slides](2026/CYBERAR2026_CTI_proactiva_ciberdefensa.pptx)

### Cazando lo que no sabés que no sabés: threat hunting guiado por inteligencia, sobre un caso real

**Hacking Day 2026** · Paraná, Entre Ríos · *Octubre 2026*

Toda defensa opera sobre supuestos: que la telemetría ve todo, que las detecciones funcionan, que si algo pasa alguien lo va a ver. Esta charla presenta el threat hunting guiado por inteligencia como un proceso disciplinado para desafiar esos supuestos, usando la matriz de Rumsfeld como mapa y un caso real de la región: un usuario pegó en Win+R un comando desde un señuelo ClickFix y la única alerta llegó casi cinco minutos después, por una IP que ya estaba en una lista de reputación. Recorremos la hunt completa, desde la hipótesis sobre la clave RunMRU hasta las seis queries en SentinelOne que reconstruyen la cadena (MSI silencioso, binarios .NET inyectados y una extensión falsa de Chrome como persistencia), y cerramos con cómo escribir el informe: qué tiene que decir, para quién, y por qué una hunt sin hallazgos también es un resultado si podés explicar por qué tu query habría encontrado algo.

[Slides](2026/Cazando_lo_que_no_sabes_que_sabes.pptx) · Plantilla de informe: [ES](Templates/Informe_Threat_Hunting__01-2026_-ClickFix_RunMRU.pdf) · [EN](Templates/Threat_Hunting_Report_Template_-_ClickFix_RunMRU_EN.pdf)

### Writing the Incident Story: Professional Reporting for Malware and Cyber Attacks

**Deathcon Cordoba** · *Noviembre 2026*

Cómo redactar reportes de incidentes claros, precisos y accionables para casos de malware y ciberataques, con foco en la comunicación profesional dentro del proceso de Incident Response.

> 🗓️ Charla confirmada. Material en preparación; se publicará después del evento (noviembre 2026).

---

## 2024

### Inteligencia de Amenazas: la importancia en las organizaciones

**Security Meet** · Colonia, Uruguay · *Mayo 2024*

Un recorrido por la disciplina de Cyber Threat Intelligence (CTI) y sus diferencias con otros tipos de inteligencia, y por qué hoy dejó de ser un opcional para convertirse en un must dentro de las organizaciones.

[Slides](2024/Security%20Meet%20-%20Inteligencia%20de%20Amenazas%20-%20La%20importancia%20en%20las%20organizaciones.pdf)

---

## Plantillas

Material reutilizable que acompaña a las charlas, en la carpeta [Templates](Templates).

**Informe de Threat Hunting** · Plantilla completa con un caso de ejemplo real (ClickFix / RunMRU), presentada en Hacking Day 2026: resumen ejecutivo, hipótesis de caza, alcance y limitaciones, MITRE ATT&CK, metodología PEAK, hallazgos, plan de acción y anexos con IOCs, queries S1QL y línea de tiempo. TLP:CLEAR, con los datos de la víctima anonimizados.

[Español](Templates/Informe_Threat_Hunting__01-2026_-ClickFix_RunMRU.pdf) · [English](Templates/Threat_Hunting_Report_Template_-_ClickFix_RunMRU_EN.pdf)

---

## Licencia y uso

El material aquí publicado se comparte con fines educativos y de divulgación. Si reutilizás algo, agradezco la atribución correspondiente.
