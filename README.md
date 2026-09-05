\# cameia-auditoria



Microservicio de Auditoría y medición de CAMEIA. Mantiene el registro funcional de consumo y costo de IA, procesa eventos de forma idempotente y publica el estado operativo de cuota.



> \*\*Estado:\*\* repositorio base creado. No se informó un issue específico de implementación para el Sprint 1; por tanto, este README documenta límites arquitectónicos y no declara funcionalidad terminada.



\## Alcance actual



\- Preparar una fuente oficial para el servicio Java de auditoría.

\- Separar el ledger funcional de las trazas técnicas almacenadas en Langfuse.

\- Evitar que otros microservicios implementen contadores incompatibles.



La implementación debe comenzar únicamente después de que Jira defina alcance, contratos y responsable.



\## Responsabilidades previstas



\- Registrar eventos de consumo con una clave de idempotencia.

\- Consolidar consumo, costo y estado de cuota por periodo.

\- Mantener la verdad eventual del ledger operativo.

\- Publicar cambios de cuota mediante contratos aprobados.

\- Relacionar registros con trazas sin duplicar prompts o respuestas completas.



No es Langfuse, no almacena credenciales y no decide planes comerciales nominales.



\## Contexto arquitectónico



```mermaid

flowchart LR

&#x20;   S\[Servicios CAMEIA] -. consumo .-> R\[RabbitMQ]

&#x20;   R --> A\[cameia-auditoria]

&#x20;   A --> DB\[(PostgreSQL Auditoría)]

&#x20;   A -. estado de cuota .-> R

&#x20;   S -. trazas técnicas .-> L\[Langfuse]

&#x20;   A -. referencia de trazas .-> L

```



\## Tecnología prevista



| Elemento | Línea base |

|---|---|

| Lenguaje | Java 21 |

| Framework | Spring Boot 4.1.1 |

| Build | Maven; wrapper pendiente de confirmar |

| Persistencia | PostgreSQL 16, base/rol propios |

| Mensajería | RabbitMQ |

| Observabilidad relacionada | Langfuse autohospedado, como plataforma separada |



\## Reglas de datos y mensajería



\- Cada evento debe ser idempotente.

\- No almacenar contraseñas, tokens, CV, audio, prompts o transcripciones completas.

\- Cuentas define el límite nominal del plan; Auditoría mantiene consumo y estado eventual.

\- Los nombres y esquemas de eventos deben versionarse antes de implementar productores/consumidores.



\## Ejecución local



```text

Issue técnico: pendiente de crear/asignar

Instalación: pendiente

Pruebas: pendiente

Build: pendiente

Inicio: pendiente

Health check: pendiente

```



\## Calidad esperada



\- Pruebas de idempotencia, duplicación, orden y reentrega.

\- Pruebas de cuota, concurrencia y límites cuando estén aprobados.

\- Validación de ausencia de datos sensibles en logs y trazas.

\- CI solo después de confirmar comandos y alcance.



\## Contribución



\- `main` es estable y solo recibe promociones `develop → main` mediante Merge commit.

\- `develop` integra ramas `CM-<numero>-<descripcion-kebab-case>` mediante Squash.

\- Todo cambio ordinario entra mediante PR y revisión distinta del autor; la rama `CM-\*` se elimina después.



\## Cuándo actualizar este README



Actualizarlo en el mismo PR que cambie propósito, stack, comandos, variables, ledger, eventos, cuota, relación con Langfuse, persistencia, pruebas, despliegue o responsables. Si no aplica, justificarlo en el PR.

