Ports are mixtures of very different subregions - berths, container yards, tank farms, channels, warehouses, urban edges - so a single average is not good.

Most natural object here is not "one-polygon -> one vector", but a hierarchy:

$$
z_{part} = \text{Pool}_{i \in \text{patches in part}} \phi([e_i, \tilde{x}_i, \tilde{y}_i])
$$

$$
z_{port} = \text{Pool}_{j \in \text{patches\ in\ part}}\phi([z_{part,j}, \tilde{g}_j])
$$

where $e_i$ is the AlphaEarth embedding, $(\tilde x_i, \tilde y_i)$ are normalized coordinates inside part or port, and $g_j$ are simple geometry features for each part like area/aspect ratio/centroid.

# Aggregation

Simple averaging of all the patches within a port will throw away too much information. In this repository is a notebook that performs averaging with diversity and prototype histograms for each part, which can then be aggregated into a representation for the whole port.

## Approach

1. `patch_group_descriptors`: Takes all patch embeddings inside one part and summarises them into one vector for that part (weighted equally). Computes mean direction, concentration, standard deviation and histogram, and return these four concatenated together.

2. `build_group_descriptors` - Runs the above for each part. (In the toy dataset, this is six)

3. `aggregate` - Aggregate each of the part descriptors into one vector for one port. (In the toy dataset, this is two)

## Validation

- The toy dataset computes embeddings using two years' data for the same port. A basic validation step would be ensuring a port's 1-nearest neighbour is the same port from another year. This is observed to be exactly the case in our dataset.