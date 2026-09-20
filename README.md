Abrego Islas Iván

Proyecto final Ciencia de Datos

__Modalidad:__ A) Estudio de caracterización/epidemiológico.
Datos: Simulados, a partir de Synthea (https://synthetichealth.github.io/synthea/). También buscaré utilizar ETL-Synthea para cargar los datos a un esquema OMOP CDM.

__Pregunta:__
Dado que mi proyecto de tesis trata de pacientes epilépticos, usaré Synthea para generar una cohorte de pacientes de este tipo. Particularmente, me interesaría explorar la evolución temporal de variables fisiológicas y buscar correlaciones con el progreso de la enfermedad.
Generé algunos datos de prueba y veo que condiciones que entrarían en mi cohorte son History of seizure (situation), Seizure disorder (disorder) & Epilepsy (disorder), cada una de mayor gravedad que la anterior. Esto me permitiría evaluar pacientes que no evolucionaron hasta un diagnóstico de epilepsia y analizar si sus variables fisiológicas se mantienen más estables que la cohorte de mayor gravedad.
También me interesa utilizar las observaciones de QALY y DALY para evaluar la calidad de vida de los pacientes conforme la enfermedad progresa y relacionarlo con el esquema de tratamiento recibido: definir subgrupos a partir de qué medicamentos antiepilépticos toman y el impacto de éstos en la calidad de vida de los pacientes.
Finalmente, si genero una población con un rango de edad amplio, probablemente podría obtener varias observaciones a lo largo de la vida de cada paciente. Tener suficientes mediciones previas a una fecha índice (diagnóstico de epilepsia), me permitiría explorar la posibilidad de implementar una pregunta de la modalidad B) Modelo predictivo clínico. Por ejemplo, probabilidades predictivas del desenlace fatal de los pacientes a partir de observaciones cardíacas o predicción de síntomas experimentados a partir de un cambio de medicamento. Esto lo consideraré como un extra opcional, que dependerá de qué tantas mediciones alrededor de la fecha índice pueda generar y principalmente de qué tanto tiempo me tome responder las preguntas relacionadas con la modalidad A).

__Entregable esperado:__
- Repositorio con historial de commits realista
- Repositorio realista con herramientas aprendidas en el curso: main protegida, pre-commits, github actions, .gitignore
- Workflow de github actions con CI, Pytest, Ruff
- Análisis implementado a modo de paquetería, con entorno containerizado utilizando docker.
- Definición de cohortes en SQL sobre OMOP
- Diccionario de datos
- Sesión de limitaciones honesta
- Delcaración de uso de agentes de ia
- Cero credenciales y cero datos identificables en el repositorio 

__Para reproducir el análisis:__

	Opción 1: Generar los datos localmente
Se recomienda trabajar con un administrador de paqueterías (e.g. Anaconda navigator). Es necesario contar con Java Development Kit (JDK) 17+. 

Ejemplo de instalación:
conda install -c conda-forge openjdk=25.0.2

Confirmar que se se cuenta con JDK 17+ mediante:

java --version

También es necesario contar con Git para clonar el repositorio de Synthea:

git clone https://github.com/synthetichealth/synthea.git

Una vez clonado, buscar el archivo synthea/src/main/resources/synthea.properties y modificar las siguientes lineas:

- exporter.fhir.export = false
- exporter.csv.export = true
- physiology.generator.enabled = true
- exporter.metadata.export = false
- exporter.hospital.fhir.export = false
- exporter.symptoms.csv.export = true
- exporter.symptoms.text.export = true
- exporter.symptoms.mode = 1

Finalmente correr el generador:

gradlew.bat run -Dorg.gradle.jvmargs="-Xmx6g" --args="-p 10000 --keep=keep_epilepsy.json -t 1"

__Nota:__ Buscaba generar únicamente 10k pacientes epilépticos, pero no configuré correctamente el keep_epilepsy.json y obtuve ~11k de los cuales ~450 son epilépticos. Aún estoy definiendo si mantener así la generación y correrla varias veces para aumentar la n de epilépticos sin perder los datos de cohorte no epiléptica, ya que esto me permitiría buscar una cohorte control.



