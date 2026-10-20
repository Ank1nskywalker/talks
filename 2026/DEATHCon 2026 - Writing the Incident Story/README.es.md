# Writing the Incident Story

**Informes profesionales de malware y ciberataques** · DEATHCon 2026 · 13 y 14 de noviembre · Online + Córdoba

*[English version](README.md)*

Un workshop autoguiado para aprender a escribir informes de threat hunting que alguien pueda usar. Cazás una intrusión ClickFix en Microsoft Sentinel con queries semilla, y lo que encontrás es la materia prima del informe: resumen ejecutivo primero, hechos separados de evaluaciones y un nivel de confianza en cada hallazgo.

La caza es el medio. **El informe es el entregable.**

---

## Qué hay en esta carpeta

| | |
|---|---|
| [Slides (ES)](slides/Writing%20the%20Incident%20Story%20%28ES%29.pdf) · [PPTX](slides/Writing%20the%20Incident%20Story%20%28ES%29.pptx) | La charla, con notas del orador |
| [Slides (EN)](slides/Writing%20the%20Incident%20Story%20%28EN%29.pdf) · [PPTX](slides/Writing%20the%20Incident%20Story%20%28EN%29.pptx) | The talk in English |
| [queries/](queries/) | Las queries semilla, una por archivo, listas para pegar en Sentinel |
| [rubrica.md](rubrica.md) | La lista contra la que se revisa tu informe |
| [Informe modelo y plantilla](https://github.com/Ank1nskywalker/Templates) | Cómo se ve un informe terminado, en español e inglés (Word y PDF) |

---

## Antes de empezar

- **Acceso al lab.** Usá el acceso al lab de Microsoft Sentinel que te da DEATHCon. Las queries usan `TimeGenerated`; en el advanced hunting de Defender XDR, cambialo por `Timestamp`.
- **La plantilla.** Bajá el informe modelo del repo [Templates](https://github.com/Ank1nskywalker/Templates), en [español](https://github.com/Ank1nskywalker/Templates/blob/main/Plantilla%20Informe%20Threat%20Hunting%20-%20ClickFix%20RunMRU%20-%20Sentinel%20%28ES%29.docx) o en [inglés](https://github.com/Ank1nskywalker/Templates/blob/main/Threat%20Hunting%20Report%20Template%20-%20ClickFix%20RunMRU%20-%20Sentinel%20%28EN%29.docx). Mantené la estructura y reemplazá el contenido con tu propia caza.
- **No hace falta saber KQL.** Las queries ya están escritas. El tiempo se va en leer resultados y escribir, no en pelear con la sintaxis.

---

## La hipótesis

> Si alguien pegó un comando desde un señuelo ClickFix, vamos a ver un intérprete lanzado por Explorer (Win+R) o por el navegador (FileFix) con un comando de descarga y, segundos después, la cadena que ese comando dispara.

| | |
|---|---|
| **Fuente de datos** | Eventos de procesos, red, archivos y registro del EDR en todo el parque: los eventos, no las alertas |
| **La confirma** | Un host con un intérprete que descarga y ejecuta, seguido de la cadena que lanza |
| **La refuta** | Cero resultados, con las queries ya validadas por el control positivo |
| **Brecha conocida** | El portapapeles no se registra. RunMRU solo ve Win+R; un pegado en Terminal (Win+X) solo lo ve Script Block Logging |
| **ATT&CK** | T1204.004 → T1059.001 · T1564.003 · T1218.007 · T1055 · T1176.001 |

---

## El ejercicio

1. **Mirá la charla** o leé las slides. Prestá atención a la parte de escritura: es lo que se revisa.
2. **Corré primero el control positivo** ([Q0a](queries/00a-table-coverage.kql), [Q0b](queries/00b-reporting-hosts.kql)). Confirmá que las tablas tienen datos y qué hosts reportan. Cero resultados sin control positivo no prueban nada.
3. **Corré las queries de caza en orden.** Cada resultado abre la pregunta siguiente. Cuando una query encuentra algo, anotá el host y la hora y usalos para acotar la próxima.
4. **Clasificá cada resultado** como malicioso, benigno o no concluyente, y escribí por qué. El lab es compartido con otros workshops, así que vas a ver actividad que parece maliciosa y no lo es. El ruido que documentás también es parte de la historia.
5. **Escribí el informe** con la plantilla. El resumen ejecutivo va primero y abre con el veredicto. Marcá el TLP en la portada.
6. **Revisalo contra la [rúbrica](rubrica.md)** y compartilo en el canal del workshop en el Discord de DEATHCon. Lo revisamos con la misma rúbrica.

No hace falta que encuentres todo. Un informe honesto sobre lo que encontraste y lo que no pudiste ver vale más que una página de capturas.

### Queries semilla

| # | Pregunta | Qué busca | Tabla | Archivo |
|---|---|---|---|---|
| 0a | ¿Puedo ver? | Datos en cada tabla de la que depende la caza | Device* | [00a](queries/00a-table-coverage.kql) |
| 0b | ¿Qué hosts? | Hosts que envían telemetría de Defender | DeviceInfo | [00b](queries/00b-reporting-hosts.kql) |
| 1 | ¿Alguien pegó un comando? | Explorer o el navegador lanzan un intérprete con un comando de descarga | DeviceProcessEvents | [01](queries/01-lineage.kql) |
| 1b | ¿Fue el cuadro Ejecutar? | Explorer escribe el comando en RunMRU | DeviceRegistryEvents | [01b](queries/01b-runmru.kql) |
| 2 | ¿Por qué no lo vio? | Un script que oculta su propia consola | Event (4104) | [02](queries/02-hidden-console-4104.kql) |
| 2b | Lo mismo, sin 4104 | Los cmdlets que registró el sensor alrededor del hallazgo de Q1 | DeviceEvents | [02b](queries/02b-powershellcommand.kql) |
| 3 | ¿Qué vino después? | msiexec lanza un PowerShell oculto | DeviceProcessEvents | [03](queries/03-msiexec-hidden-powershell.kql) |
| 4 | ¿Con quién habló? | Un binario .NET (csc, ilasm…) con conexión de red | DeviceNetworkEvents | [04](queries/04-dotnet-outbound.kql) |
| 5 | ¿Se ubicó? | Un proceso que no es navegador consulta su IP pública | DeviceNetworkEvents | [05](queries/05-public-ip-check.kql) |
| 6 | ¿Cómo se queda? | Un proceso que no es navegador modifica el perfil del navegador | DeviceFileEvents | [06](queries/06-browser-preferences.kql) |
| 7 | Próxima caza | Un manifest de extensión creado fuera del navegador | DeviceFileEvents | [07](queries/07-manifest-json.kql) |

Los comentarios de las queries están en inglés. Miran 30 días hacia atrás (`ago(30d)`): si las corrés después del evento, ampliá la ventana.

### Lo que te va a enseñar el lab

- **No todas las fuentes se ingieren.** Si el log operativo de PowerShell (4104) no está en la tabla `Event`, Q2 no puede encontrar nada. Usá Q2b y escribí la brecha en *Alcance y Limitaciones*. Una brecha declarada es un hallazgo; una brecha oculta es un falso negativo.
- **Los eventos de red pueden venir agregados.** Las conexiones cortas pueden aparecer solo como `ConnectionSuccessAggregatedReport`, reportadas minutos después. Por eso Q4 no filtra por `ActionType`.
- **Los navegadores escriben su perfil con un archivo temporal y un rename.** Un cambio en `Preferences` puede verse como `FileRenamed`. Por eso Q6 tampoco filtra por `ActionType`.
- **Un resultado vacío es un resultado** solo si el control positivo funcionó. Decí cuál de los dos fue.

---

## Cómo se ve un buen informe

El [informe modelo](https://github.com/Ank1nskywalker/Templates) aplica todo lo de la charla a un caso real de ClickFix (TLP:CLEAR, datos de la víctima anonimizados): resumen ejecutivo BLUF, hipótesis con criterios de confirmación y refutación, alcance y limitaciones, hallazgos con veredicto y nivel de confianza (ICD 203), hechos separados de evaluaciones, ACH para hipótesis en competencia, plan de acción con responsables y anexos con IOCs, queries re-ejecutables, línea de tiempo y evidencia de controles.

Usalo como referencia, no como formulario: tu caza va a encontrar otras cosas, y tu informe lo tiene que decir.

---

## Referencias

- Splunk SURGe, [PEAK Threat Hunting Framework](https://www.splunk.com/en_us/blog/security/peak-threat-hunting-framework.html) (2023)
- D. J. Bianco, [The Pyramid of Pain](https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html) (2013)
- ODNI, [ICD 203: Analytic Standards](https://www.dni.gov/files/documents/ICD/ICD-203.pdf)
- MITRE ATT&CK, [T1204.004 User Execution: Malicious Copy and Paste](https://attack.mitre.org/techniques/T1204/004/)
- mrd0x, [FileFix: a ClickFix alternative](https://mrd0x.com/filefix-clickfix-alternative/)
- Microsoft, [DeviceRegistryEvents](https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-deviceregistryevents-table) · [PowerShell logging on Windows](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_logging_windows)

---

David Pérez · [@anakinswal](https://x.com/anakinswal) · Material compartido con fines educativos. Si lo reutilizás, agradezco la atribución.
