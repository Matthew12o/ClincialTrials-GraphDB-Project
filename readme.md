# Clinical Trial GraphDB Overlay Project
****
## 1. Purpose

The project aims to overlay the ClinicalTrials.org database into a graphDB for analyzing unique relationships between the PIs, Research Facility, Sponsors, Study, Intervention and Disease Area. Compared to regular relational database (SQL), graphDB allows for better representation of business relationships within the  dataset. The graphDB will make insights on competition, overlap of treatment or disease, trends and saturation of disease area much easier and faster to analyze. 
****

## 2. Sample Queries
*All samples have limited number of nodes to increase performance and may not have all relationships and nodes*

### SPONSOR MAP **ImmunoGen, Inc.**
![sponsor_map](samples/SPONSOR_MAP_IMMUNOGEN.png)

>Shows all Studies, Treatment and Conditions the company is exploring

### CONDITION MAP **Wilson Disease**
![condition_map](samples/CONDITION_MAP_WILSON_DISEASE.png)

>Identify Sponsors that are studying the disease as well as Interventions that are used to treat the disease.

### STUDY on INTERVENTION **Ultomiris** - (Active Clinical Trials highlighted)
![study_on_intervention](samples/Study%20Status%20on%20INTERVENTION%20Ultomiris.png)

>Shows clincial trials (Studies) that are evaluating the Intervention.

### Find Competition on INTERVENTION **Eculizumab** ON CONDITION **pnh**
![find_competition](samples/COMPETITION%20for%20INTERVENTION%20eculizumab%20ON%20CONDITION%20PNH.png)

>Shows all Interventions being used to treat a Condition. You can also remove Condition filter to show competition of all Conditions being treated by the Intervention

### SPONSORS on CONDITION **ALS** - (Industry Sponsor highlighted)
![sponsor_on_condition](samples/SPONSORS_on_CONDITION_ALS_INDUSTRY_ONLY.png)

>Shows all Sponsors that are exploring a Condition and any relationship between the Sponsors (Color indicates the SPONSOR is an INDUSTRY, ie pharmaceutical company)

### CONDITION Shares INTERVENTION with CONDITION **hypophosphatasia**
![share_intervention_with_other_conditions](samples/condition_share_hpp.png)

>Identify Conditions that are being treated with the Intervention of HPP. This could potentially allow you to identify additional indications that an intervention could be used

****
## 3. Data Schema

![DBSchema](/samples/Schema.png)

### Nodes:
|# | Label | Properties| Type |
|--|-------|------------|-------|
|1| Study | ||
|||ID |Integer|
|||completion_date|Date|
|||created_at|DateTime|
|||nct_id|String|
|||official_title|Text|
|||overall_status|String|
|||phase | String|
|||source | String|
|||start_date| Date|
|||studyId | Integer|
|||updated_at | DateTime|
|2|Sponsor||||
|||SponsorId| Integer|
|||name|String|
|||sponsorId|Integer|
|3|Facility*|||
|4|PI*|||
|5|Intervention|||
|||InterventionId|Integer|
|||name|String|
|||type|String|
|6|Condition|||
|||ConditionId|Integer|
|||name|String|
|7|Disease Area*||
|8|Region*|||

   
### Relationships:
|#|Relationship Name|Originating Node|Target Node|Preferred Search Direction|Relationship Type|Properties|Data Type|
|-|--------|--------|------|------|-----|----|---|
|1|STUDY_ON|Study|Condition|Unidirectional|Direct||
|2|EVALUATES|Study|Intervention|Unidirectional|Direct|||
|3|LEADS|Sponsor|Study|Unidirectional|Direct|||
|4|COLLABORATES_ON|Sponsor|Study|Unidirectional|Direct||
|5|COLLABORATES_WITH|Sponsor|Sponsor|Bidirectional|Indirect||
|||||||occurance|Integer|
|6|STUDIES|Sponsor|Condition|Unidirectional|Direct||
|||||||occurance|Integer|
|7|INVESTIGATES|Sponsor|Intervention|Unidirectional|Direct||
|||||||occurance|Integer|
|8|TREATS|Intervention|Condition|Unidirectional|Direct||
|||||||occurance|Integer|
|9|ALIAS|Intervention|Intervention|Bidirectional|Direct/Indirect|||

> Direct Relationship indicates relationship built into the SQL database's schema

> Indirect Relationship indicates relationship that was inferred based on the relationship the node has with one or more nodes


****

## 4. Sources

ClinicalTrials.org : Information on clinical trials

Proprietary Dataset : Additional contextual datasets are used to clean and tag the ClinicalTrials.org database to build relationships and speedup query

International Classification of Disease (ICD)* : Classify conditions

****

## 5. System Setup 
1. SQL Database : PostGRE SQL
2. GraphDB : Neo4j
3. DB Conversion Algorithem : Proprietary
4. Analytics Engine : Proprietary
5. Visualization Platform : Neo4j Bloom

****

## 6. Development Plan
1. Prototype

ClinicalTrials.org SQL database to be mapped into a graphDB.

2. V.1
   
Proprietary data and tagging to be added to improve performance and ease of search.

3. V.2

NLP implemented to clean the ClinicalTrails.org data further

FDA's Device and Drug database to be overlayed on INTERVENTION for additional context

ICD database is overlayed on top of the CONDITION nodes to better group the disease groups

4. V.3

Bloomberg BI database to be overlayed on top of the Sponsor/Intervention data to better contextualize drug development and analyst expectations on individual drugs