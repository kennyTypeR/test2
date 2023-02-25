# test2
from sklearn.cluster import AgglomerativeClustering, Birch, DBSCAN, KMeans, MeanShift, MiniBatchKMeans, OPTICS, SpectralClustering, AffinityPropagation, FeatureAgglomeration, AgglomerativeClustering, Birch, DBSCAN, KMeans, MeanShift, MiniBatchKMeans, OPTICS, SpectralClustering
from sklearn.mixture import GaussianMixture
from sklearn.metrics import silhouette_score
<pre>

# Create a list of clustering algorithms to test
clustering_algorithms = [
    KMeans(random_state=42),
    MiniBatchKMeans(random_state=42),
    AffinityPropagation(),
    MeanShift(),
    SpectralClustering(random_state=42),
    AgglomerativeClustering(),
    DBSCAN(),
    OPTICS(),
    Birch(),
    GaussianMixture(random_state=42),
    FeatureAgglomeration(),
    # Add more clustering algorithms here
]

# Loop over clustering algorithms and number of clusters and print silhouette score
for algo in clustering_algorithms:
    for n_clusters in range(4, 10):
        if isinstance(algo, GaussianMixture):
            algo.set_params(n_components=n_clusters)
            algo.fit(X)
            cluster_labels = algo.predict(X)
        elif isinstance(algo, FeatureAgglomeration):
            algo.set_params(n_clusters=n_clusters)
            cluster_labels = algo.fit_predict(X)
        else:
            cluster_labels = algo.set_params(n_clusters=n_clusters).fit_predict(X)
        # Compute the silhouette score
        score = silhouette_score(X, cluster_labels)
        # Print the algorithm name, number of clusters, and silhouette score
        print(f"{type(algo).__name__} ({n_clusters} clusters): {score}")
</pre>
