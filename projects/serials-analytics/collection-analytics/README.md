# Collection analytics: where have all the Country life magazines gone? #

## The problem - are the magazines used where we think they are? ##

Most library material operates as a 'floating' collection, shifting across all 57 branches as needed. In contrast, magazines are treated as 'fixed' assets tied to a home branch to maintain sequence. As magazines are browsing collections, the availability is important. However, this creates a paradox. We optimise for the storage of magazines rather than their discoverability across the network.

Because checkout data is overwritten and location history is transient, it is difficult to determine whether a magazine has low use because the title is unpopular, or simply because it is located at the wrong branch. We are managing a systemic discovery problem with siloed tools. Using the Intelligent Materials Management System (IMMS), you can trace movements at item or bib level, but not report on all copies at one time. The Library Management System(LMS) allows reporting to item level, but is amnesiac.

## The solution ##

To resolve this problem, I developed a tracking tool that treats magazine collections as dynamic data points. By capturing snapshots of the collection and cross-referencing with branch demographics, circulation requests and checkout history, the system can indicate 'interest fit' between a title and its home branch. This enables us to allocate more effectively and identify magazines that would be better placed at a different location. It also can help optimise the number of subscriptions held of each title. 

## Architectural approach ##

The system is built on a modular schema designed to cope with the complexities of serial publications and the large library network.

**Controlled Vocabulary & Data Normalisation** 

Title relationships have standardised mapping using ISSNs (rather than internal system IDs) to ensure continuity even when titles have not yet been catalogued. This faciliatates reporting and planning as the collection changes.

**Cost-Aware Modelling** 

Incorporates contract pricing to calculate cost-per-use, providing a clear financial view that the Council Insights team’s high-level snapshots do not include.

**Historical Context** 

Maintains a history of branch closures, re-openings, and subscription shifts, allowing users to understand data against known periods of inactivity. Tracking temporary closures and capacity changes helps to plan the subscription spend wisely.

**Integrated Demographics** 

References demographic data from the local board level to provide context for branch-specific serial interest.

**Capability management**

Records which magazine sub-collections a branch may hold (e.g. Children's magazines). While the LMS may have a location code for a sub-collection at a branch, there may be no such mapping on the IMMS.

## Technical implementation ##
To focus on high-impact insights, I have compromised on some details such as individual issue freight costs. Instead, I prioritised more easily calculable metrics such as individual subscription cost and VAS estimates set at contract level. 

**Structural fidelity** 

Records existing LMS and Council financial identifiers for legibility and cross-system data integrity. Previously, this information was dispersed across multiple systems.

**Data enrichment** 

Moves beyond basic checkout counts by categorising titles by subject and target audience (e.g., community language subsets, youth collections).

**KPI management** 

Includes item creation date and issue designation. The database is structured so that these can be queried which helps to track supplier performance, allowing for data-backed contract negotiations.

## Outcomes and future refinements ##

**Operational efficiency** 

Enables a repeatable reporting cycle (fortnightly) that reduces the manual burden on the Insights team.

**Evidence-based collection development** 

Transitions collection management from ad-hoc spreadsheet tracking to an interrogated, historicised database. LMS records are restricted to the current and previous financial year. This database will enable long-term tracking.

**Systemic insight** 

Provides a granular view of title lifecycle, helping to determine when to redeploy, increase, or discontinue subscriptions.