# OntoMamI
OntoMamI: Towards a Semantic Interoperability Ontology-Framework for Breast Cancer
<img width="82" height="20" alt="image" src="https://github.com/user-attachments/assets/ac236fd9-5abd-4ba8-ab42-0e8dd848fb22" />

OntoMamI is a minimum viable prototype (MVP) of an operational ontology between ontologies for breast cancer, integrated with BI-RADS and connected to HL7 FHIR Foundation. The goal is to facilitate interoperability between clinical ontologies and imaging data (BI-RADS), with support for FHIR R5 serialization, DICOM reading, synthetic record generation (Diego), reasoning, and SPARQL queries.

Table of Contents
Objective and Scope
Repository Structure
Requirements and Dependencies
Installation and Configuration
Basic Use
Pipelines and Workflows
Contribution
License and Synthetic Data
Implementation Notes and Decisions
Contact
Objective and Scope
Provide an MVP for OntoMamI focused on mammograms and BI-RADS.
Support FHIR R5 for serialization of clinical resources.
Offer an OWL (Turtle) skeleton with modules and alignments to SNOMED CT, NCIt, LOINC, RxNorm, and ICD-11.
Include basic DICOM reading and generation of synthetic records (Diego) in JSON and RDF.
Prepare reasoning (OWL reasoners) and SPARQL queries for validations.
Maintain an open structure for future extensions (other breast cancers, biomarkers, treatments).
Repository structure
owl/

OntoMamI.owl.ttl: Initial OWL skeleton in Turtle.
namespaces.ttl: Prefixes and base IRIs.
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

diego_synthetic.json: Synthetic record in FHIR JSON.
diego_synthetic.ttl: Issued in RDF/Turtle.
dicom/

read_dicom_example.py: Base Python script for reading DICOM metadata.
dicom_mapping.md: DICOM → FHIR RDF mapping guide.
reasoning/

shacl/ontobci_profile_shacl.ttl: SHACL shapes for profile validation.
sparql/competencia_base.rq: SPARQL query templates for competition.
rdf_store/

setup_instructions.md: Guide for deploying an RDF store and loading artifacts.
docs/

plan.md: Implementation and progress plan.
decisions.md: Record of design decisions.
.gitignore

README.md (current adapted file)

Requirements and dependencies
Java (for OWL/RDF tools and potential reasoners such as HermiT or Pellet).
Python 3.x (for DICOM scripts and pipelines).
Node.js (optional, for JSON processing utilities and FHIR tooling).
Pydicom/DCMTK (for DICOM reading).
FHIR libraries for testing (e.g., HAPI FHIR if using Java, or fhir.resources for Python).
A compatible RDF store (Virtuoso, Stardog, Blazegraph, or similar) for SPARQL and reasoning.
SHACL validators (pySHACL or OWL SHACL tooling).
Notes: The artifacts in this MVP are designed to run in a Python environment with optional Node for FHIR. It can be adapted to a CI/CD flow in GitHub Actions.

Installation and configuration (proposal)
Environment recommendation
Create a Conda environment (optional) for Python and RDF tools.
Install DICOM and RDF dependencies.
Initial steps

Clone the repository:
git clone https://github.com/EdgarMex/OntoMamI.git
cd OntoMamI
Set up environments and dependencies (optional with conda):
conda create -n ontomami python=3.11
conda activate ontomami
pip install pydicom rdflib pyshex shapely prck [adjust as needed]
Test DICOM reading:
python dicom/read_dicom_example.py path_to_sample_dicom.dcm
Generate Diego in JSON:
python -m diego generate_synthetic --output diego_synthetic.json
or use the examples provided diego/diego_synthetic.json
Validate SHACL:
pySHACL orients shapes against RDF/Turtle data.
SPARQL queries:
Use a local SPARQL endpoint or Virtuoso to run competencia_base.rq against diego_synthetic.ttl.
Note on versions

This MVP uses FHIR R5 for profiles and examples. StructureDefinitions are in fhir_profiles/r5/StructureDefinitions/.
Diego examples are available in JSON and RDF/Turtle for interoperability.
Basic usage
OWL and SHACL structure verification

Validate ontology and profile consistency with reasoners (HermiT/Pellet/ELK).
Run SHACL to see that the RDFs comply with the defined shapes.
Artifact generation

DICOM: metadata reading and ImagingStudy/Observation generation.
Diego: FHIR resource generation for a BI-RADS-centered synthetic record.

RDF: TTL conversion of FHIR resources for storage.
Validation and queries

SPARQL: execute queries on generated RDF/Turtle.
SHACL: validate FHIR and RDF structures against OntoMamI definitions.
Contribution
To contribute, follow these basic guidelines:

Open Issues to report problems, features, or improvements.
Create Pull Requests describing the proposed change.
Maintain a clear development branch (e.g., main for the stable version and develop for integration).
Ensure that relevant tests or validations pass before merging.
Quick PR review guide:

Ensure consistency of names and paths (OntoMamI).
Verify that modifications do not break the existing structure.
Include tests or validation cases where applicable.
License and Contribution:

This project is licensed under MIT. See the License section for more details.
License and synthetic data
License: MIT
Synthetic data: Diego and other artifacts are simulated examples and not real data. Their use is governed by the MIT license and the repository guidelines.
Implementation notes and decisions
Versions: FHIR R5 selected.
Focus: mammograms/BI-RADS as the main MVP.
Extensibility: the structure supports future modules for other breast cancers, biomarkers, and treatments.
Useful links
OntoMamI on GitHub (public): https://github.com/EdgarMex/OntoMamI

Internal project documentation (docs/): plan.md, decisions.md
Artifact files: owl, fhir_profiles, diego, dicom, reasoning, rdf_store
Contact
MVP author: EdgarMex
Communication channel: repository Issues and Pull Requests
Email (optional): contact@example.com
Final notes

Check that the license badge is displayed correctly in the header.
Verify that the links point to the correct OntoMamI repository.
If you want, I can adapt the content to add usage examples, installation steps with exact GitHub Actions commands for CI/CD, or a file index with full paths. Would you like me to add any of these?

Yes, add exact GitHub Actions commands for CI/CD
Sider Fusion
Perfect! I'll add a proposed CI/CD workflow for OntoMamI using GitHub Actions. It includes:

Structure validation and basic README linting.
Validation of key files (LICENSE, CODE_OF_CONDUCT.md, README.md).
Basic RDF/OWL validation using an available command line client (robolectric? no). To avoid heavy dependencies, we will do a simple validation: check that TTL/Owl files exist and are not empty; optionally run a Python script if you want to validate RDF lite.
Optional tests: if you have tests, they can be run; here is a skeleton so you can add them.
CI/CD plan with GitHub Actions

