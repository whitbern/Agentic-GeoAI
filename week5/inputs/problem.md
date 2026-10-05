# 🌍 Broad research problem

How can GeoAI help us understand how urban spatial structure affects the spread of
classification errors in proximity-based spatial association networks?

## Initial boundary conditions

- The problem must contain a meaningful spatial component.
- The investigation should be achievable with public or research-accessible data.
- The aim is to produce a testable research question, not merely a descriptive mapping exercise.
- The methodology must contain an explicit validation strategy.
- All "individuals" in the model are synthetic agents or building-level nodes. No real persons,
  identities, or individual-level records are used or inferred.
- No real targeting, strike, or casualty data is used. The study measures model reliability
  under controlled synthetic error, not real-world outcomes.
- Scope is a single-semester class project: two study areas, one primary modeling method,
  one controlled error-injection experiment.

## Human notes

### Domain knowledge
- Pattern-of-life and association-based decision support tools infer relationships partly from
  spatial proximity (who lives near whom, who shares a building or block). A classification error
  on one node can propagate to spatially associated nodes.
- Hypothesis: the same error rate produces different propagation patterns depending on urban
  spatial structure. Dense, irregular, highly connected layouts are expected to amplify and spread
  errors. Sparse, regular grids are expected to contain them. The direction and size of the
  effect are the empirical question.
- Statistically this is a moderation question: spatial structure moderates the relationship
  between injected error rate and error reach.
- Known spatial threats to validity that the workflow must address explicitly:
  - IID violations / spatial autocorrelation
  - Spatial leakage between training and test data
  - Edge effects at study-area boundaries
  - MAUP (sensitivity to proximity thresholds and aggregation units)


### Candidate study areas
- Dense, irregular: Kibera, Nairobi. Among the best-mapped informal settlements in OSM
  (community-mapped through the Map Kibera project). Cross-check building completeness
  against Google Open Buildings and document the OSM snapshot date.
- Sparse, regular baseline: a US planned suburban grid (e.g., a Phoenix or Las Vegas suburb).


### Constraints
- Python only. Libraries: OSMnx, GeoPandas, Shapely, NetworkX, PySAL (libpysal, esda, spreg).
  ArcGIS Pro is available for visualization and checks.
- First time using OSMnx and NetworkX together, so the workflow should be well commented
  and modular.
- Prefer explainable methods (spatial autoregressive models, network metrics, tree-based
  models) over deep learning. A GNN is optional and only if time allows.
- Fully reproducible: fixed random seeds, pinned package versions, a dated data snapshot,
  and a version-controlled GitHub repository.


### Datasets I already know
- OpenStreetMap building footprints and street networks (via OSMnx; historical via ohsome)
- Microsoft Global ML Building Footprints / Google Open Buildings (cross-check OSM completeness)
- WorldPop or GHSL population grids (optional, to weight nodes by population)

### Papers to treat as evidence
- Boeing (2017), OSMnx — methods for acquiring and analyzing street networks
- Zhang, Song, Luo & Wu (2023), "Geocomplexity explains spatial errors," IJGIS
- Janowicz et al. (2020), "GeoAI: spatially explicit artificial intelligence techniques…," IJGIS
- Li et al. (2024), "GeoAI for Science and the Science of GeoAI," JOSIS
- Roberts et al. (2017), cross-validation strategies for data with spatial structure, Ecography
- Anselin (1995), Local Indicators of Spatial Association (LISA)
- Openshaw (1984), The Modifiable Areal Unit Problem
- Bhavnani et al. (2014), "Group Segregation and Urban Violence," AJPS (spatial ABM precedent)
- Weidmann & Salehyan (2013), "Violence and Ethnic Segregation," ISQ (GIS-coded ABM precedent)
- Weber (2016), "Keep adding," Environment and Planning D (databases and targeting)
- Suchman (2020), "Algorithmic warfare and the reinvention of accuracy," Critical Studies on Security

