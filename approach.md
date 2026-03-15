Ports are mixtures of very different subregions - berths, container yards, tank farms, channels, warehouses, urban edges - so a single average is not good.

Most natural object here is not "one-polygon -> one vector", but a hierarchy:

$$
z_{part} = Pool_{i \in patches\ in\ part}\phi([e_i,\tilde x_i, \tilde y_i])
$$
$$
z_{port} = Pool_{j \in patches\ in\ part}\phi([z_{part,j}, \tilde g_j])
$$
where $e_i$ is the AlphaEarth embedding, $(\tilde x_i, \tilde y_i)$ are normalized coordinates inside part or port, and $g_j$ are simple geometry features for each part like area/aspect ratio/centroid.

# Aggregation

Simple averaging of all the patches within a port will throw away too much information. In this repository is a notebook that performs averaging with diversity and prototype histograms for each part, which can then be aggregated into a representation for the whole port.

## Approach

Describe execution of `build_group_descriptors`


