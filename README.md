[![DOI](https://zenodo.org/badge/1166953335.svg)](https://doi.org/10.5281/zenodo.18776591)

This repository contains the JSON schemas for the Sinai Manuscripts Data Portal and Digital Library. Included, as well, are processing scripts for generating the compiled versions of the schemas for each object and entity type from their constituent parts.

# Sinai Schemas

The `schema` directory contains the individual schemas and sub-schemas that define and document the Sinai Manuscripts Data Portal data model. It is recommended that users of these schemas have a basic familiarity with the [JSON Schema specification](https://json-schema.org/). Sinai schemas use the [Draft 2020-12](https://json-schema.org/draft/2020-12) version of the spec. For the purposes of organizing the schemas, as well as reducing redundancy, common sub-schemas are defined in distinct JSON schema files and referenced by URI in parent schemas as needed. (In JSON Schema terms, this is called schema composition, and Sinai schemas use [modular structuring](https://json-schema.org/understanding-json-schema/structuring) via the `$id` and `$ref` keywords).

Most schema files' file name are self-explanatory, e.g. the `bib.json` file contains a schema defining the data model for bibliographic references used throughout object and entity types. References to these sub-schemas will use the URI of that file: `"$ref": "https://raw.githubusercontent.com/UCLALibrary/sinai_schemas/main/schema/bib.json"`.


Two files, `utils.json` and `enums.json` operate slightly differently. Each of these contain a list of defined sub-schemas (in the `$defs` section); these individual sub-schemas are referenced by parent schemas, not the `utils.json` schema itself. For example, the common pattern for entries in controlled term lists is defined in the `utils.json` as `#/$defs/controlled_term` (the use of `#` represents the 'root' of the JSON document in the JSON Pointer syntax used by JSON Schema). References to these sub-schemas will append the JSON Pointer fragment to the `utils.json` URI, e.g. `"$ref": https://raw.githubusercontent.com/UCLALibrary/sinai_schemas/main/schema/utils.json#/$defs/controlled_term"`. It should be noted that there was not a consistent design reasoning for when an independent JSON file was created for a sub-schema rather than an entry in the utils' `$defs` section. However, the referencing parent schema will make clear what file it is referencing.

The `enums.json` file, on the other hand, contains enumerated/controlled term lists that are even more strictly controlled. For example, the well-defined and stable list of manuscript states (i.e., `https://raw.githubusercontent.com/UCLALibrary/sinai_schemas/main/schema/enums.json#/$defs/states`). Uses may notice that these term lists _first_ define the list as a controlled term (referencing the `utils.json` file via `$ref`) and then use the `enums` property to specify the closed list of allowable values (or in this case `id`/`label` pairs).

## Compiled Schemas

Within the `schema/out` subdirectory are the compiled versions of these schemas in single documents for each record type (objects and entities):
- Manuscript Objects: `ms_obj.compiled.json`
- Layers: `layer.compiled.json`
- Text Units: `text_unit.compiled.json`
- Agents: `agent.compiled.json`
- Works: `work.compiled.json`
- Places: `place.compiled.json`

These files are produced via the JavaScript compiler script found in `scripts/compile-schemas`. Once compiled, these files are self-contained (i.e., they do not rely on HTTP queries to resolve sub-schema references), and, as such, should be used to validate JSON data records against.

## SMDL Schema

The schema for records exported and aggregated for ingest into the Sinai Manuscripts Digital Library is defined in the `schema/smdl.json` file. As this file _only_ contains sub-schemas defined internally (via the `$defs` field and referenced using the JSON Pointer syntax adopted by the JSON Schema spec), no additional compilation is needed for this schema. In other words, it can be used as-is to validate against.

# Processing Scripts

Processing and related utility scripts may be found in the `scripts` directory. The `scripts/compile-schemas` directory contains a NodeJS processing script used to compile the object and entity schemas from their constituent parts. Please see the associated README file in that directory for installation and usage instructions.
