# test2
<pre>
# Set the figure size
fig_size = (4, 4)

# Set the random state range
random_states = range(1000)

# Create a DataFrame to store the results
results = pd.DataFrame(columns=['Method', 'Random State', 'Silhouette Score', 'Calinski-Harabasz Score', 'Davies-Bouldin Score'])

# Loop through each random state value and plot the scatter chart
n_rows = len(random_states)
n_cols = 3
fig, axes = plt.subplots(nrows=n_rows, ncols=n_cols, figsize=(n_cols*fig_size[0], n_rows*fig_size[1]))
for i, random_state in enumerate(random_states):
    # Fit KMeans to the standardized data using PCA, FastICA, and NMF
    for j, (method_name, method) in enumerate([('PCA', PCA(n_components=2)),
                                               ('FastICA', FastICA(n_components=2)),
                                               ('NMF', NMF(n_components=2))]):
        # Fit the method to the standardized data
        data_transformed = method.fit_transform(data)

        # Fit KMeans to the transformed data
        kmeans = KMeans(n_clusters=n_clusters, random_state=random_state)
        kmeans.fit(data_transformed)

        # Evaluate the KMeans performance using the clustering metrics
        silhouette = silhouette_score(data_transformed, kmeans.labels_)
        ch_score = calinski_harabasz_score(data_transformed, kmeans.labels_)
        db_score = davies_bouldin_score(data_transformed, kmeans.labels_)

        # Add the results to the DataFrame
        results = results.append({'Method': method_name,
                                  'Random State': random_state,
                                  'Silhouette Score': silhouette,
                                  'Calinski-Harabasz Score': ch_score,
                                  'Davies-Bouldin Score': db_score},
                                 ignore_index=True)

        # Plot the scatter chart
        ax = axes[i][j]
        ax.scatter(data_transformed[:, 0], data_transformed[:, 1], c=kmeans.labels_)
        ax.set_xlabel('Component 1')
        ax.set_ylabel('Component 2')
        ax.set_title(f'{method_name}, Random State {random_state}\nSilhouette Score: {silhouette:.2f}')
        ax.axis('square')

# Adjust spacing between subplots
fig.tight_layout()

# Save the results to a new CSV file
results.to_csv('kmeans_results.csv', index=False)
</pre>
