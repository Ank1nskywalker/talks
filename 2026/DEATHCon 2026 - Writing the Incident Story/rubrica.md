# Rúbrica · Writing the Incident Story

*[English version](rubric.md)*

Ocho controles antes de entregar un informe. No son de estilo: cada uno responde a uno de los lectores de la charla (quien decide, el SOC, el próximo hunter y el auditor). Usala para revisar tu propio informe antes de compartirlo en Discord; nosotros lo revisamos con la misma lista.

Cada control se califica como **Cumple**, **Parcial** o **Falta**.

| # | Control | Lector | Cumple | Falla típica |
|---|---|---|---|---|
| 1 | El resumen ejecutivo entra en una página y abre con el veredicto | Quien decide | La primera oración dice qué pasó y con qué confianza. Alguien que no lee KQL puede decidir solo con esa página | Arranca con la metodología, o el veredicto aparece en el tercer párrafo |
| 2 | Cada hallazgo tiene veredicto, evidencia y nivel de confianza | SOC | Cada hallazgo dice malicioso, benigno o no concluyente, apunta a la query y al evento que lo sostienen, y usa lenguaje calibrado (ICD 203) | "Se observó actividad sospechosa", sin veredicto ni confianza |
| 3 | Los hechos están separados de las evaluaciones | Todos | Lo que muestra la telemetría y lo que inferís de ella están escritos por separado y se distinguen de un vistazo | "csc.exe fue inyectado" escrito como hecho cuando la telemetría solo muestra una conexión |
| 4 | Las limitaciones y brechas están escritas, no implícitas | Próximo hunter, auditor | Las fuentes no ingeridas, los puntos ciegos de las queries y el ruido excluido están listados, con su impacto en la conclusión | Una query vacía reportada como "no se encontró nada" sin decir si la fuente existía |
| 5 | Cada recomendación tiene prioridad, responsable y estado | Quien decide | Cada acción se puede asignar mañana: qué, quién, qué tan urgente y si ya está hecha | "Mejorar el monitoreo" |
| 6 | Las queries del anexo se pueden volver a correr tal cual | Próximo hunter | Queries completas, con ventana temporal y plataforma, que devuelven lo mismo al pegarlas | Capturas de resultados, o queries recortadas a la línea interesante |
| 7 | Los datos de la víctima están anonimizados | Auditor | Usuarios, hostnames, IPs internas y nombres de la organización reemplazados por placeholders; los IOCs del atacante se mantienen | Un usuario o hostname real en una captura o en la línea de tiempo |
| 8 | El TLP está marcado en la portada | Todos | El nivel TLP está en la portada y es coherente con lo que el informe contiene | Sin TLP, o TLP:CLEAR en un informe con datos internos sin redactar |

**Una caza sin hallazgos también se reporta.** Si tus queries volvieron vacías, el informe dice cuáles, por qué confiás en ese resultado (control positivo) y qué no pudiste ver. Eso es un informe completo.
