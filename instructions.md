Overview
--------

- The dataset is a sample of 100 ports.
- Top-level structure: dict keyed by port UN/LOCODE (unique port ID).

Data keys you should use
-----------------------

1) `sample_dict[port_code]['final_data_wo_details']`
   - Provides port facility polygons (berths + anchorage). 
   - Use this for embedding aggregation.
   - Ignore polygons with `source == 'gfw_data'` (known to be unreliable).

2) `sample_dict[port_code]['port_boundary_polygon']`
   - Convex hull around the port + surrounding congestion zone.
   - Contains large open-sea area; not recommended as the primary aggregation region.

3) `sample_dict[port_code]['port_metadata']`
   - Static port attributes (optional support for validation).
   
GCP data access
---------------

- Bucket: `alphaearth_annual_embed_2025`
- GCP project: `tau-agentic-priors`
- Action: coordinate with Ram to set up bucket access.

Primary task (embed aggregation)
--------------------------------

1. Goal: generate a single meaningful port-level embedding for each port.
2. Avoid naive mean over `port_boundary_polygon`; it is dominated by open water and collapses port differences.
3. Focus on facility pixels from `final_data_wo_details` as the primary embedding source.

Design requirements
-------------------

- Select aggregation unit(s): per facility polygon, per berth zone, or aggregated facility region.
- Choose functions: robust aggregate stats (mean, median, variance, top-k, weighted average, etc.).
- Consider unsupervised/semi-supervised strategies:
  - cluster embeddings at port/facility level
  - reduce dimensionality within facility areas before aggregation
  - compare to historical port classes (size/region/type)
- Define quality metrics and validation:
  - silhouette score, cluster purity, intra-group vs inter-group distance
  - confirm similar ports are nearby in embedding space
  - confirm different ports are separated

Deliverable
-----------

- Port-level embedding set with clear aggregation logic and quality validation.
- Documentation of method, units, and metrics.
- Short report on why this is better than global hull mean.
