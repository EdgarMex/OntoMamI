# OntoMamI
OntoMamI: Towards a Semantic Interoperability Ontology-Framework for Breast Cancer

OntoMamI: Ontología para Mamografía BI-RADS
License
<img width="82" height="20" alt="image" src="https://github.com/user-attachments/assets/f66b450f-89ac-47d4-b65f-bd53385c42b0" />

OntoMamI es un prototipo mínimo viable (MVP) de una ontología operativa entre ontologías para Cáncer de Mama, integrada con BI-RADS y conectada a HL7 FHIR Foundation. El objetivo es facilitar la interoperabilidad entre ontologías clínicas y datos de imageología (BI-RADS), con soporte para serialización FHIR R5, lectura de DICOM, generación de expedientes sintéticos (Diego), razonamiento y consultas SPARQL.

Tabla de contenidos
Objetivo y Alcance
Estructura del repositorio
Requisitos y dependencias
Instalación y configuración
Uso básico
Pipelines y flujos de trabajo
Contribución
Licencia y datos sintéticos
Notas de implementación y decisiones
Contacto
Objetivo y Alcance
Proveer un MVP para OntoMamI centrado en mamografías y BI-RADS.
Soportar FHIR R5 para serialización de recursos clínicos.
Ofrecer un esqueleto OWL (Turtle) con módulos y alineaciones a SNOMED CT, NCIt, LOINC, RxNorm e ICD-11.
Incluir lectura básica de DICOM y generación de expedientes sintéticos (Diego) en JSON y RDF.
Preparar razonamiento (OWL reasoners) y consultas SPARQL de competencia para validaciones.
Mantener una estructura abierta para futuras extensiones (otros cánceres de mama, biomarcadores, tratamientos).
Estructura del repositorio
owl/

OntoMamI.owl.ttl: Esqueleto OWL inicial en Turtle.
namespaces.ttl: Prefijos y IRIs base.
fhir_profiles/

r5/
StructureDefinitions/
BreastCancer-Observation-BI_RADS.json
BreastCancer-Bundle.json
BI_RADS_CategoryCode.json
Breast_ReceptorStatus.json
examples/
BI-RADS4_example.json
Diego_DiagnosticReport.json
diego/

diego_synthetic.json: Expediente sintético en FHIR JSON.
diego_synthetic.ttl: Expedido en RDF/Turtle.
dicom/

read_dicom_example.py: Script base en Python para leer metadatos DICOM.
dicom_mapping.md: Guía de mapeo DICOM → FHIR RDF.
reasoning/

shacl/ontobci_profile_shacl.ttl: SHACL shapes para validación de perfiles.
sparql/competencia_base.rq: Plantillas de consultas SPARQL de competencia.
rdf_store/

setup_instructions.md: Guía para desplegar un RDF store y cargar artefactos.
docs/

plan.md: Plan de implementación y progreso.
decisions.md: Registro de decisiones de diseño.
.gitignore

README.md (archivo actual adaptado)

Requisitos y dependencias
Java (para herramientas OWL/RDF y potential reasoners como HermiT o Pellet).
Python 3.x (para scripts DICOM y pipelines).
Node.js (opcional, para utilidades de procesamiento JSON y tooling FHIR).
Pydicom/DCMTK (para lectura de DICOM).
Librerías FHIR para pruebas (p. ej., HAPI FHIR si usas Java, o fhir.resources para Python).
Un RDF store compatible (Virtuoso, Stardog, Blazegraph, o similares) para SPARQL y razonamiento.
SHACL validators (pySHACL u OWL SHACL tooling).
Notas: Los artefactos de este MVP están diseñados para ser ejecutados en un entorno con Python y Node opcional para FHIR. Se puede adaptar a un flujo CI/CD en GitHub Actions.

Instalación y configuración (propuesta)
Recomendación de entorno

Crear un entorno Conda (opcional) para Python y herramientas RDF.
Instalar dependencias de DICOM y RDF.
Pasos iniciales

Clonar el repositorio:
git clone https://github.com/EdgarMex/OntoMamI.git
cd OntoMamI
Configurar entornos y dependencias (opcional con conda):
conda create -n ontomami python=3.11
conda activate ontomami
pip install pydicom rdflib pyshex shapely prck [ajuste según necesidades]
Probar lectura DICOM:
python dicom/read_dicom_example.py path_to_sample_dicom.dcm
Generar Diego en JSON:
python -m diego generate_synthetic --output diego_synthetic.json
o usar los ejemplos proporcionados diego/diego_synthetic.json
Validar SHACL:
pySHACL orients shapes contra los datos RDF/Turtle.
Consultas SPARQL:
Usar un endpoint SPARQL local o Virtuoso para ejecutar competencia_base.rq contra diego_synthetic.ttl.
Nota sobre versiones

Este MVP utiliza FHIR R5 para perfiles y ejemplos. Los StructureDefinitions están en fhir_profiles/r5/StructureDefinitions/.
Los ejemplos de Diego están disponibles en JSON y RDF/Turtle para interoperabilidad.
Uso básico
Verificación de estructuras OWL y SHACL

Validar consistencia de ontología y perfiles con reasoners (HermiT/Pellet/ELK).
Ejecutar SHACL para ver que los RDF cumplen con las shapes definidas.
Generación de artefactos

DICOM: lectura de metadatos y generación de ImagingStudy/Observation.
Diego: generación de recursos FHIR para un expediente sintético centrado en BI-RADS.
RDF: conversión TTL de los recursos FHIR para almacenamiento.
Validación y consultas

SPARQL: ejecutar competencias sobre RDF/Turtle generado.
SHACL: validar estructuras FHIR y RDF frente a definiciones de OntoMamI.
Contribución
Para contribuir, sigue estas pautas básicas:

Abrir Issues para reportar problemas, características o mejoras.
Crear Pull Requests describiendo el cambio propuesto.
Mantener una rama de desarrollo clara (por ejemplo, main para la versión estable y develop para integración).
Asegurarse de que las pruebas o validaciones relevantes pasen antes de fusionar.
Guía rápida de revisión de PR:

Asegurar consistencia de nombres y rutas ( OntoMamI ).
Verificar que las modificaciones no rompan la estructura existente.
Incluir tests o casos de validación cuando aplique.
Licencia y Contribución:

Este proyecto está licenciado bajo MIT. Ver sección de Licencia para más detalles.
Licencia y datos sintéticos
Licencia: MIT
Datos sintéticos: Diego y otros artefactos son ejemplos simulados y no datos reales. Su uso se rige por la licencia MIT y las indicaciones del repositorio.
Notas de implementación y decisiones
Versiones: FHIR R5 seleccionado.
Enfoque: mamografías/BI-RADS como MVP principal.
Extensibilidad: la estructura soporta futuros módulos para otros cánceres de mama, biomarcadores y tratamientos.
Enlaces útiles
OntoMamI en GitHub (publico): https://github.com/EdgarMex/OntoMamI
Documentación interna del proyecto (docs/): plan.md, decisions.md
Archivos de artefactos: owl, fhir_profiles, diego, dicom, reasoning, rdf_store
Contacto
Autor del MVP: EdgarMex
Canal de comunicación: repository Issues y Pull Requests
Correo (opcional): contact@example.com
Notas finales

Revisa que el badge de licencia se muestre correctamente en la cabecera.
Verifica que los enlaces apunten al repositorio OntoMamI correcto.
