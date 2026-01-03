# SOC Base – Detección y mejora de visibilidad en endpoint Windows

## Contexto
Durante una revisión de actividad sospechosa en un endpoint Windows, se detectó un **intento de ejecución de un archivo desde un directorio escribible por el usuario**.  
El análisis se realizó **sin herramientas externas (SIEM/EDR)**, utilizando únicamente capacidades nativas del sistema operativo.

## Rol
Analista **SOC junior**, responsable de la detección inicial, el análisis de evidencias disponibles y la propuesta de mejoras defensivas.

## Alcance del análisis
Incluye:
- Endpoint Windows
- Visor de eventos de Windows
- Auditoría y directivas locales

Fuera de alcance:
- SIEM
- EDR
- Análisis avanzado de malware

## Descripción del incidente
Se produjo un **intento de ejecución** de un archivo con extensión `.exe` ubicado en una ruta no habitual para binarios legítimos (`AppData\Local\Temp`).  
El archivo no llegó a ejecutarse correctamente, pero el intento fue suficiente para iniciar una investigación.

## Investigación y hallazgos
1. **Revisión inicial de registros**
   - Registros de *Aplicación* y *Sistema*: sin eventos relevantes asociados al intento.
   - Registro de *Seguridad*: no se observaban eventos de creación de procesos.

2. **Identificación de una carencia**
   - Se determinó que **la auditoría de creación de procesos no estaba habilitada**, lo que limitaba la visibilidad ante intentos de ejecución.

3. **Acción correctiva**
   - Se habilitó la **auditoría de creación de procesos**.
   - Tras repetir el intento, se validó la aparición del **evento 4688** en el registro de Seguridad.

4. **Mejora adicional**
   - Durante la investigación se identificó que los eventos 4688 **no incluían la línea de comandos**.
   - Se habilitó la directiva para **incluir la línea de comandos en los eventos de creación de procesos**, mejorando el contexto disponible para análisis futuros.
   - Se validó que los nuevos eventos 4688 incluyen dicha información.

## Conclusión
El incidente puso de manifiesto una **limitación de visibilidad** en el endpoint que impedía una investigación adecuada.  
Mediante la habilitación de auditoría y la mejora del nivel de detalle de los eventos, el SOC logró **aumentar la capacidad de detección y análisis** ante intentos de ejecución sospechosos.

Este laboratorio demuestra:
- Capacidad de análisis con visibilidad limitada
- Identificación de carencias defensivas
- Implementación y validación de mejoras
- Enfoque realista de SOC junior

## Evidencias
Las capturas incluidas en el repositorio documentan:
- Intento de ejecución desde una ruta sospechosa
- Habilitación de la auditoría de creación de procesos
- Evento 4688 generado tras la corrección
- Inclusión de la línea de comandos en los eventos


---

# SOC Base – Detection and visibility improvement on a Windows endpoint

## Context
During a review of suspicious activity on a Windows endpoint, an **attempt to execute a file from a user-writable directory** was detected.  
The analysis was performed **without external tools (SIEM/EDR)**, using only native Windows features.

## Role
**Junior SOC analyst**, responsible for initial detection, evidence analysis, and defensive improvement recommendations.

## Scope of analysis
Includes:
- Windows endpoint
- Windows Event Viewer
- Local audit and policy settings

Out of scope:
- SIEM
- EDR
- Advanced malware analysis

## Incident description
An **execution attempt** of a `.exe` file located in an uncommon path (`AppData\Local\Temp`) was observed.  
The file did not execute successfully, but the attempt was sufficient to start the investigation.

## Investigation and findings
1. **Initial log review**
   - Application and System logs: no relevant events related to the attempt.
   - Security log: no process creation events were initially present.

2. **Identified gap**
   - Process creation auditing was **not enabled**, limiting visibility for execution attempts.

3. **Corrective action**
   - Process creation auditing was enabled.
   - After repeating the attempt, **event 4688** was successfully generated in the Security log.

4. **Additional improvement**
   - It was identified that process creation events **did not include command-line information**.
   - A policy was enabled to **include command-line data in process creation events**, improving analysis context.
   - The change was validated by reviewing new 4688 events.

## Conclusion
This incident revealed a **lack of visibility** on the endpoint that limited proper investigation.  
By enabling auditing and improving event detail, the SOC increased its ability to **detect and analyze suspicious execution attempts**.

This lab demonstrates:
- Analysis with limited visibility
- Identification of defensive gaps
- Implementation and validation of improvements
- A realistic junior SOC approach

## Evidence
The included evidence shows:
- Execution attempt from a suspicious path
- Process creation auditing enabled
- Generated event 4688
- Command-line information included in the event
