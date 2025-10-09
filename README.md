# OntoMamI: Towards a Semantic Interoperability Ontology-Framework for Breast Cancer
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
Email: ecastillo@uaslp.mx
Final notes

Verify that the links point to the correct OntoMamI repository.

Structure validation and basic README linting.
Validation of key files (LICENSE, CODE_OF_CONDUCT.md, README.md).
Basic RDF/OWL validation using an available command line client (robolectric? no). To avoid heavy dependencies, we will do a simple validation: check that TTL/Owl files exist and are not empty; optionally run a Python script if you want to validate RDF lite.
Optional tests: if you have tests, they can be run; here is a skeleton so you can add them.
CI/CD plan with GitHub Actions
Enable CI on pushes to main and PRs to main.
Jobs:
setup: Install Python and optional dependencies.
lint-readme: Verify the presence of README.md and license badge.
validate-files: Confirm LICENSE, CODE_OF_CONDUCT.md, README.md, and expected structures.
optional-tests: Placeholder for RDF/OWL tests if you add tests.
File: .github/workflows/ci-ontomami.yml (proposed content)

yaml
name: OntoMamI CI

on: 
push: 
branches: 
-main 
pull_request: 
branches: 
- main

jobs: 
validate-structure: 
runs-on: ubuntu-latest 
steps: 
- name: Checkout 
uses: actions/checkout@v3 

- name: Set up Python 
uses: actions/setup-python@v5 
with: 
python-version: '3.11' 

- name: Validate essential files exist 
run: | 
set -e 
REQUIRED_FILES=(LICENSE CODE_OF_CONDUCT.md README.md) 
for f in "${REQUIRED_FILES[@]}"; do 
if [ ! -f "$f" ]; then echo "Missing $f"; exit 1; fi 
donated 
echo "All required files present" 

- name: Validate README badge reference 
run: | 
if ! grep -q "https://img.shields.io/badge/License-MIT-blue.svg" README.md; then 
echo "Warning: MIT badge not found in README. Consider adding a LICENSE badge." 
# exit 1 # uncomment if you want to demand the badge 
fi 
echo "README badge check completed" 

lint-readme: 
runs-on: ubuntu-latest 
steps: 
- name: Checkout 
uses: actions/checkout@v3 

- name: Lint README (simple sanity) 
run: | 
python -m pip install --upgrade pip 
python -m pip install PyYAML # if you need parsing 
# Simple example: verify that there is a table of contents 
if ! grep -qi "^##" README.md; then
echo "README.md: No clearly highlighted sections (header indicators)."
exit 1
fi
echo "README sanity check passed"

optional-tests:
runs-on: ubuntu-latest
if: always() # always runs; can be disabled if there are no tests
steps:
- name: Checkout
uses: actions/checkout@v3

- name: Set up Python
uses: actions/setup-python@v5
with:
python-version: '3.11'

- name: Install dev dependencies (optional)
run: |
python -m pip install --upgrade pip
# Add dependencies if you have RDF validation scripts
# e.g., rdflib, pySHACL, click, etc.
pip install rdflib pySHACL

- name: Run optional RDF/OWL validations (example)
run: |
echo "Running optional validations (customize as needed)"

# Example: verify that TTL/owl is not empty
for f in owl/*.ttl; do
if [ -s "$f" ]; then
echo "Validated $f (non-empty)"
else
echo "Warning: $f is empty"
exit 1
fi
done

Notes and Recommendations

This workflow is a starting point. You can improve it with specific tools:
RDF validation with PySHACL if you define SHACL shapes (pip install pyshacl) and run a script that validates your RDFs.
Lint for Turtle/OWL with tools like rapper (Raptor) or rapper-cli if you want stricter RDF validation.
Add a release job:
Create a release when merging to main (v0.1.0 tag). You can use actions/create-release and actions/upload-release-asset to package artifacts.
Security:
Make sure you don't expose credentials in workflows. Use GitHub Actions secrets if you need access to services (Virtuoso, etc.).

