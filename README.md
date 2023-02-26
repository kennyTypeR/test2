# test2
<pre>
for random_state in random_states:
    # Fit PCA to the standardized data
    pca = PCA(n_components=2, random_state=random_state)
    data_pca = pca.fit_transform(data)

    # Fit KMeans to the principal components
    kmeans = KMeans(n_clusters=n_clusters, random_state=random_state)
    kmeans.fit(data_pca)

    # Evaluate the KMeans performance using the clustering metrics
    silhouette = silhouette_score(data_pca, kmeans.labels_)
    ch_score = calinski_harabasz_score(data_pca, kmeans.labels_)
    db_score = davies_bouldin_score(data_pca, kmeans.labels_)

    # Add the results to the DataFrame
    results = results.append({'Random State': random_state,
                              'Silhouette Score': silhouette,
                              'Calinski-Harabasz Score': ch_score,
                              'Davies-Bouldin Score': db_score},
                             ignore_index=True)

    # Plot the scatter chart
    fig, ax = plt.subplots(figsize=fig_size)
    ax.scatter(data_pca[:, 0], data_pca[:, 1], c=kmeans.labels_)
    ax.set_xlabel('PC1')
    ax.set_ylabel('PC2')
    ax.set_title(f'KMeans Clustering with Random State {random_state}')
    plt.show()

# Save the results to a new CSV file
results.to_csv('kmeans_results.csv', index=False)
</pre>
