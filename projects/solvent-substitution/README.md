# Solvent substitution #

## Project summary ##
A system designed to facilitate safer chemical selection for conservation applications achieved by modelling molecular properties, regulatory hazards and compliance measures.


## Architecture ##


![System Architecture](./../../images/solvents-erd.png)


The system applies a normalised relational schema to manage the lifecycle of a chemical.
- Solvents are treated separately from their formulas to allow the system to model complex products that contain multiple chemical components.

- Every property, hazard, and classification is linked to a reference entity, ensuring that decisions are auditable and justifiable.

## Data hierarchy ##
Tables are logically grouped into three functional layers:

| Layer | Key Tables | Role |
|-------|------------|------|
|`Operational` | `solvent`, `alias`, `solvent_formula` | Tracks the "What" — the agent being used.|
| `Scientific` | `formula`, `property_type`, `observation` |Tracks the "Properties" — experimental data and molecular qualities. |
| `Compliance` | `hazard_code`, `p_code`, `hazard_classification` |Tracks the "Safety" — GHS/regulatory handling requirements. |

## Data dictionary ##

### Data schema conventions ###
To ensure consistency across data, the following attributes are present in most tables:

- id: Primary Key
- notes: Text field for context
- archived_at: Timestamp (Null = Active, Present = Archived/Soft-deleted)


| Table |Column | Logic |
|-------|-------|-------|
|`observation` | `priority_score` |Used to weight experimental data when conflicting sources exist.|
|`formula` | `smiles / inchi` | Standardized molecular representation for structural analysis.|
|`solvent_formula` | `fraction` | Enables mixture modelling (e.g., a solvent that is 70% A and 30% B).|
|`hazard_code` | `h_code` |The standard GHS Hazard code (e.g., H225) for regulatory mapping.|

*(Standard global fields `id`, `notes`, `archived_at` apply)*



<details>
<summary>Click to view full table schema.</summary>
## Table `solvent`

Represents a usable solvent entity. 
May correspond to a pure chemical, a mixture of several chemicals 
or a specific concentration. 
Acts as the primary operational unit in the system.

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `solvent_id` | `text` | Primary |
| `canonical_name` | `text` |  Nullable |
| `cas_number` | `text` |  Nullable |
| `refchem_id` | `text` |  Nullable |
| `is_mixture` | `bool` |  Nullable |
| `note` | `text` |  Nullable |
| `is_deleted` | `bool` |  Nullable |
| `deleted_at` | `timestamp` |  Nullable |

## Table `reference`

Bibliographic or documentary sources, including SDS/MSDS, literature, and datasets. May support solvents, aliases, or observations.

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `reference_id` | `text` | Primary |
| `title` | `text` |  Nullable |
| `reference_type` | `text` |  Nullable |
| `url` | `text` |  Nullable |
| `publisher` | `text` |  Nullable |
| `jurisdiction` | `text` |  Nullable |
| `version` | `text` |  Nullable |
| `published_date` | `date` |  Nullable |

## Table `alias`

Alternative names for a solvent, including common names, IUPAC names, abbreviations, and trade names.

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `alias_id` | `text` | Primary |
| `solvent_id` | `text` |  |
| `alias_name` | `text` |  |
| `alias_type` | `text` |  Nullable |

## Table `property_type`

Controlled vocabulary defining measurable properties (e.g. boiling_point, flash_point, Hansen parameters).

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `property_code` | `text` | Primary |
| `property_name` | `text` |  Unique |
| `default_unit` | `text` |  Nullable |

## Table `observation`

Stores literature-reported measurements for solvent properties. 
Multiple values per property may exist across different references and conditions.

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `observation_id` | `text` | Primary |
| `solvent_id` | `text` |  |
| `property_code` | `text` |  Nullable |
| `value` | `numeric` |  |
| `unit` | `text` |  Nullable |
| `conditions` | `text` |  Nullable |
| `reference_id` | `text` |  Nullable |
| `notes` | `text` |  Nullable |
| `priority_score` | `int4` |  Nullable |

## Table `formula`

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `formula_id` | `text` | Primary |
| `canonical_name` | `text` |  |
| `molecular_formula_hill` | `text` |  Nullable |
| `molecular_weight` | `numeric` |  Nullable |
| `molar_vol` | `numeric` |  Nullable |
| `smiles` | `text` |  Nullable |
| `inchi` | `text` |  Nullable |
| `boiling_point` | `numeric` |  Nullable |
| `flash_point` | `numeric` |  Nullable |
| `vapour_pressure_min_20c` | `numeric` |  Nullable |
| `vapour_pressure_max` | `numeric` |  Nullable |
| `tlv_ppm` | `numeric` |  Nullable |
| `liquid_density_at_20c` | `numeric` |  Nullable |
| `water_solubility_g_l_at_20c` | `numeric` |  Nullable |
| `water_solubility_boolean` | `numeric` |  Nullable |
| `logp_min` | `numeric` |  Nullable |
| `logp_max` | `numeric` |  Nullable |
| `viscosity_cp_at_20c` | `numeric` |  Nullable |
| `notes` | `text` |  Nullable |
| `standard_data_source` | `text` |  Nullable |
| `variant` | `text` |  Nullable |

## Table `solvent_formula`

Links solvents to one or more formulas. 
Supports mixtures via multiple rows per solvent.

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `solvent_id` | `text` | Primary |
| `formula_id` | `text` | Primary |
| `fraction` | `numeric` |  Nullable |

## Table `p_code`

Precautionary statements (P-codes) describing recommended handling, storage, and response actions.

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `p_code_id` | `text` | Primary |
| `p_code` | `text` |  Nullable |
| `phrase` | `text` |  |
| `category` | `text` |  Nullable |
| `notes` | `text` |  Nullable |

## Table `pcode_group`

Logical grouping of precautionary codes for organisational purposes.

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `group_id` | `text` | Primary |
| `group_name` | `text` |  Nullable |
| `description` | `text` |  Nullable |

## Table `hazard_code`

Links hazard statements to associated precautionary statements.

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `hazard_id` | `text` | Primary |
| `h_code` | `text` |  Nullable |
| `hazard_statements` | `text` |  Nullable |

## Table `hazard_classification`

Provides classification metadata for hazards, including hazard class, category, and signal word.

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `classification_id` | `text` | Primary |
| `hazard_id` | `text` |  Nullable |
| `hazard_class` | `text` |  Nullable |
| `category` | `text` |  Nullable |
| `signal_word` | `text` |  Nullable |

## Table `hazard_pcode`

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `hazard_id` | `text` | Primary |
| `pcode_id` | `text` | Primary |

## Table `pcode_group_map`

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `group_id` | `text` | Primary |
| `p_code_id` | `text` | Primary |

## Table `solvent_reference`

Associates references with solvents at a general level (e.g. SDS documents).

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `solvent_id` | `text` | Primary |
| `reference_id` | `text` | Primary |

## Table `alias_reference`

Associates references with specific aliases, enabling product- or name-specific documentation.

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `alias_id` | `text` | Primary |
| `reference_id` | `text` | Primary |

## Table `formula_hazard`

Associates intrinsic hazards with chemical formulas.

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `formula_id` | `text` | Primary |
| `hazard_id` | `text` | Primary |

## Table `solvent_hazard`

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `solvent_id` | `text` | Primary |
| `hazard_id` | `text` | Primary |

</detail>