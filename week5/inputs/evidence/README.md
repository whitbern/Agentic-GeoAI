## Supporting Evidence
- This is just more reasoning for some of the assumptions I think I want to make in the paper. 

### Claim 1. Network structure shapes how things spread through it
- Watts & Strogatz (1998), "Collective dynamics of 'small-world' networks," Nature.
  Clustering and path length change how fast and far things propagate.
- Pastor-Satorras & Vespignani (2001), "Epidemic spreading in scale-free networks,"
  Physical Review Letters. Degree structure determines whether spread is contained.
- Barthélemy (2011), "Spatial networks," Physics Reports. Review of how spatial
  embedding constrains network structure and dynamics.
- Ghadiri, Saramäki & Hiraoka (2026), "Epidemic reproduction numbers in spatial
  networks," arXiv:2603.22150 (Aalto University). Compares spread on Erdős–Rényi
  and random geometric graphs; attributes the difference mainly to loops of
  various lengths carrying transmission. Theoretical basis for the arrangement
  mechanism.



### Claim 2: Urban spatial structure varies measurably across cities, especially if the cities are spread out
- Boeing (2017), OSMnx. Methods for building and measuring networks from OSM.
- Boeing (2019), "Urban spatial order: street network orientation, configuration, and
  entropy," Applied Network Science. Quantifies grid vs. organic layouts across cities;
  supports the planned-grid vs. irregular comparison.
- Iotti et al. (2017), "Infection dynamics on spatial small-world network models,"
  Physical Review E. Spread on random geometric graphs vs. Watts–Strogatz and
  degree-preserving rewired versions. Methods precedent for the rewiring test;
  shows the spatial-vs-rewired difference is established, not a finding.
- Lei, Liu, Milojevic-Dupont & Biljecki (2024), "Predicting building characteristics
  at urban scale using graph neural networks and street-level context," Computers,
  Environment and Urban Systems, 102129. doi:10.1016/j.compenvurbsys.2024.102129.
  Building-level graphs as an active GeoAI method; GNNs used because they model
  spatial relationships between buildings.
- Jia, Zhang & Narahara (2024), "Characterizing residential building patterns in
  high density cities using graph convolutional neural networks," CAADRIA 2024,
  Vol. 2, pp. 39–48. Builds graphs with building footprint centroids as nodes,
  connected within a distance; precedent for this study's graph construction (Hong Kong).
- Liu & Song (2025), "Unsupervised Plot Morphology Classification via Graph
  Attention Networks: Evidence from Nanjing's Walled City," Land 14(7), 1469.
  Building centroids as nodes with Delaunay-based proximity edges; alternative


### Claim 3: Spatial structure produces systematic error in GeoAI
- Zhang, Song, Luo & Wu (2023), "Geocomplexity explains spatial errors," IJGIS.
- Roberts et al. (2017), cross-validation for data with spatial structure, Ecography.
- Openshaw (1984), The Modifiable Areal Unit Problem.
- Janowicz et al. (2020), GeoAI: spatially explicit AI, IJGIS.
- Behnisch et al. (2019), "Settlement percolation," Landscape and Urban Planning.
  Building locations connected within a distance threshold; critical distance
  for a spanning cluster. Precedent for building-point proximity graphs.
- Review of percolation-like transitions in cities and landscapes (Findings).
  Clustering distance and density as control parameters in urban percolation;
  anchors the per-site distance threshold choice.
- Cecchini, Cestnik & Pikovsky (2021), "Impact of local network characteristics
  on network reconstruction," Physical Review E. False-positive and false-negative
  rates in inferred networks depend on local structure (shortest path length,
  detour degree). Non-spatial; supports the general claim that structure shapes
  inference error.
- Sun, Hu, Lakhanpal & Zhou (2023), "Spatial Cross-Validation for GeoAI," in
  Gao, Hu & Li (eds.), Handbook of Geospatial Artificial Intelligence, CRC Press.
  Random CV overestimates performance on geographic data due to spatial
  autocorrelation; compares four spatial CV methods. Primary citation for
  spatial-leakage control.

### Claim 4: The data is adequate, as in there is enough data (and its gaps are documented)
- Map Kibera project documentation (community OSM mapping of Kibera).
- Sirko et al. (2021), "Continental-scale building detection from high resolution
  satellite imagery" (Google Open Buildings). Used to check OSM completeness.
- Okyere, Lu & Brunn (2025), "Evaluating the Quality of Open Building Datasets for
  Mapping Urban Inequality: A Comparative Analysis Across 5 Cities,"
  arXiv:2508.12872. Compares Google and Microsoft AI-derived footprints, including
  Nairobi; analyzes polygon size distributions and completeness in informal areas.
  Directly informs the footprint data gate and node-definition check.
- Herfort, Lautenbach, Porto de Albuquerque, Anderson & Zipf (2023), "A
  spatio-temporal analysis investigating completeness and inequalities of global
  urban building data in OpenStreetMap," Nature Communications 14, 3985.
  OSM building completeness across 13,189 urban areas; regions with large building
  stocks are often under-mapped; over half of Sub-Saharan African urban building
  data comes from humanitarian mapping. Basis for the data gate.
- Yeboah et al. (2021), "Analysis of OpenStreetMap data quality at different stages
  of a participatory mapping process: evidence from slums in Africa and Asia,"
  ISPRS International Journal of Geo-Information. Remote-mapping completeness
  depends on slum morphology (building density, rooftop architecture), from 84%
  to zero across sites. Flags a potential confound: under-mapping may be
  systematically worse in dense areas, biasing degree.

### Claim 5 (new): Association-based inference propagates suspicion through networks
- Suspicion scoring based on guilt-by-association, collective inference, and
  focused data access (NYU working paper). Describes propagating suspicion scores
  through a partially known association network until they stabilize. Supports the premise that erroneous seed labels degrade inference through an association network. Its averaging rule differs from this study's cascade rule; the propagation rule here is a stylised abstraction.

### Context only (motivates the problem; not evidence for the mechanism)
- Weber (2016), "Keep adding," Environment and Planning D.
- Suchman (2020), "Algorithmic warfare and the reinvention of accuracy."
- Bhavnani et al. (2014), AJPS; Weidmann & Salehyan (2013), ISQ.