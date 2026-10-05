## Supporting Evidence
- This is just more reasoning for some of the assumptions I think I want to make in the paper. 

### Claim 1. Network structure shapes how things spread through it
- Watts & Strogatz (1998), "Collective dynamics of 'small-world' networks," Nature.
  Clustering and path length change how fast and far things propagate.
- Pastor-Satorras & Vespignani (2001), "Epidemic spreading in scale-free networks,"
  Physical Review Letters. Degree structure determines whether spread is contained.
- Barthélemy (2011), "Spatial networks," Physics Reports. Review of how spatial
  embedding constrains network structure and dynamics.
- Iotti et al., Infection dynamics on spatial small-world network models (PRE 2017)
- Behnisch et al., Settlement percolation (Landscape and Urban Planning 2019)



### Claim 2: Urban spatial structure varies measurably across cities, especially if the cities are spread out
- Boeing (2017), OSMnx. Methods for building and measuring networks from OSM.
- Boeing (2019), "Urban spatial order: street network orientation, configuration, and
  entropy," Applied Network Science. Quantifies grid vs. organic layouts across cities;
  supports the planned-grid vs. irregular comparison.
- Iotti et al. (2017), "Infection dynamics on spatial small-world network models,"
  Physical Review E. Spread on random geometric graphs vs. Watts–Strogatz and
  degree-preserving rewired versions. Methods precedent for the rewiring test;
  shows the spatial-vs-rewired difference is established, not a finding.


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

### Claim 4: The data is adequate, as in there is enough data (and its gaps are documented)
- Map Kibera project documentation (community OSM mapping of Kibera).
- Sirko et al. (2021), "Continental-scale building detection from high resolution
  satellite imagery" (Google Open Buildings). Used to check OSM completeness.

### Claim 5 (new): Association-based inference propagates suspicion through networks
- Suspicion scoring based on guilt-by-association, collective inference, and
  focused data access (NYU working paper). Describes propagating suspicion scores
  through a partially known association network until they stabilize. Supports the premise that erroneous seed labels degrade inference through an association network. Its averaging rule differs from this study's cascade rule; the propagation rule here is a stylised abstraction.

### Context only (motivates the problem; not evidence for the mechanism)
- Weber (2016), "Keep adding," Environment and Planning D.
- Suchman (2020), "Algorithmic warfare and the reinvention of accuracy."
- Bhavnani et al. (2014), AJPS; Weidmann & Salehyan (2013), ISQ.