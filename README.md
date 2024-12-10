

## Context

During my stay in Busan, I noticed that my running mobile application consistently overestimated the distance I ran. The reason is likely due to noisy GPS data, caused by the presence of tunnels I went through during my run, which degraded the quality of the GPS signals.

The goal of this project is to develop a method for better estimating the actual distance of my runs. Of course, I could simply use Google Maps and its "measure distance" tool to calculate this manually in 30 seconds. Instead I spent 3 days implementing an automatic approach.

## Implementation
I framed this as a "map-matching" problem ("Cartopondance" in French). Essentially, the task involves mapping GPS points to roads and walkable areas using `OpenStreetMap` (OSM) data. Here is an example of an OSM graph, where edges are essentially representing portions of roads/paths:

![OSM Example](osm_graph_example.png)


To tackle the problem, I used a `Hidden Markov Model` (HMM) combined with the `Viterbi` algorithm. I assumed that:
- Emission probabilities follow a Gaussian distribution.
- Transition probabilities between connected nodes in the OSM graph are inversely proportional to the distance between them (with Laplace smoothing).
## Results

| **Metric**                      | **Value**     | **Difference (%)** |
|----------------------------------|---------------|---------------------|
| Estimated distance (raw GPS points)        | 11.62 km      | +38.00%            |
| Estimated distance (decoded hidden states) | 8.71 km       | +3.44%             |
| Actual distance (gold value)     | 8.42 km       | 0.00% (baseline)   |



![Map-Matching Example](output_example_1.png)

As you can see, the decoded states obtained from the ``HMM`` represents a more accurate approximation of the actual running path than raw GPS points (it "cancels" the noise):

![Map-Matching Example2](output_example_2.png)
