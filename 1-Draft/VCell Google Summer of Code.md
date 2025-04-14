# Goal
Develop a local chatbot and the web interface for querying VCell resources and provide relevant information about VCell models, modeling techniques, and using VCell software.  
Here are sample queries that the chatbot should answer:
## Exploring VCell database, e.g.  
- List all models by a certain user  
- List all models that have a specific type of geometry (e.g. analytic, constructed solid, etc), that use specific solver (e.g. CVODE), etc.  
- List all models that deal with Calcium
## Exploring individual models (using auth0 or by a user):  
- How many reactions are in a certain model? Describe mathematics, parameters, simulations.  
- What biological papers have similar modeling mechanisms?  
- Draw the reaction diagram in SBGN format
## Assist user in using VCell:  
- How to model enzymatic reaction?  
- How to define an analytic geometry?  
- How to plot a histogram for multiple simulations?  
- What do colors mean in spatial simulations plot?

## Assist a user in designing new models:  
- Generate a model for ligand-binding to receptors  
- Combine two existing models into a unified one.
## Possibly expand the project to SBML/BioPAX data, answering all the same queries
# Storage format
### JSON Metadata for VCell BioModels

| **Key**        | **Meaning**                                           | **Queriable via API**       | **Data Type**   |
| -------------- | ----------------------------------------------------- | --------------------------- | --------------- |
| `bmKey`        | Unique ID for the BioModel (used in VCML and queries) | ✅ (as `bmId`)               | `String`        |
| `name`         | Human-readable name of the model                      | ✅ (as `bmName`)             | `String`        |
| `privacy`      | Model visibility: `0 = public`, `1 = private`         | ❌                           | `Number`        |
| `groupUsers`   | Users/groups with shared access                       | ❌                           | `Array<String>` |
| `savedDate`    | Last modified timestamp (ms since epoch)              | ✅ (`savedLow`, `savedHigh`) | `Number`        |
| `annot`        | Annotation or comment, e.g., cloning history          | ❌                           | `String`        |
| `branchID`     | Internal versioning ID of the model                   | ❌                           | `String`        |
| `modelKey`     | Internal key for model definition                     | ❌                           | `String`        |
| `ownerName`    | Creator's username                                    | ✅ (as `owner`)              | `String`        |
| `ownerKey`     | Internal owner ID                                     | ❌                           | `String`        |
| `simulations`  | Array of simulation definitions                       | ❌                           | `Array<Object>` |
| `applications` | Array of simulation contexts (Apps)                   | ❌                           | `Array<Object>` |
### Inside in `simulations`

|**Key**|**Meaning**|**Queriable via API**|**Data Type**|
|---|---|---|---|
|`key`|Simulation ID (used in detailed queries)|✅ (as `simId`)|`String`|
|`branchId`|Version ID of this simulation|❌|`String`|
|`name`|Label for the simulation|❌|`String`|
|`ownerName`|Simulation creator|❌|`String`|
|`ownerKey`|Creator's internal ID|❌|`String`|
|`mathKey`|Links simulation to a math model|❌|`String`|
|`solverName`|Name of solver (e.g., IDA/CVODE, Smoldyn)|❌ (but present in sim details)|`String`|
|`scanCount`|Number of variations (e.g., parameter scans)|❌|`Number`|
|`bioModelLink`|Application and model metadata used in sim|❌|`Object`|
|`overrides`|Parameter changes specific to this sim|❌|`Array<Object>`|
### Inside `bioModelLink`

|**Key**|**Meaning**|**Queriable via API**|**Data Type**|
|---|---|---|---|
|`bioModelKey`|Same as `bmKey`|✅|`String`|
|`bioModelBranchId`|Model version used in this sim|❌|`String`|
|`bioModelName`|Name of the model|✅|`String`|
|`simContextKey`|ID for the simulation context (App)|❌|`String`|
|`simContextBranchId`|Version ID for the context|❌|`String`|
|`simContextName`|Name of context used in sim (e.g. Application0)|❌|`String`|
### Inside `overrides`

|**Key**|**Meaning**|**Queriable via API**|**Data Type**|
|---|---|---|---|
|`name`|Name of the parameter being overridden|❌|`String`|
|`type`|Override type: `"Single"` or `"List"`|❌|`String`|
|`values`|Values assigned in override|❌|`Array<String>`|
|`cardinality`|Number of override values|❌|`Number`|
### Inside `applications`

|**Key**|**Meaning**|**Queriable via API**|**Data Type**|
|---|---|---|---|
|`key`|Unique ID for application/simulation context|❌|`String`|
|`branchId`|Application version ID|❌|`String`|
|`name`|Application/context name (e.g., Application0)|❌|`String`|
|`ownerName`|Creator of the application|❌|`String`|
|`ownerKey`|Creator's internal ID|❌|`String`|
|`mathKey`|Links application to math model|❌|`String`|
