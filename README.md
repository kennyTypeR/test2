# test2
<pre>
# Set the range of random state values
random_states = range(1000)

# Set the number of clusters for KMeans
n_clusters = 5

# Set the figure size for the scatter charts
fig_size = (5, 5)

# Create a DataFrame to store the results
results = pd.DataFrame(columns=['Method', 'Random State', 'Silhouette Score', 'Calinski-Harabasz Score', 'Davies-Bouldin Score'])

# Define the methods to use
methods = [('PCA', PCA(n_components=2)),
           ('FastICA', FastICA(n_components=2)),
           ('NMF', NMF(n_components=2)),
           ('TSNE', TSNE(n_components=2, perplexity=30)),
           ('LLE', LocallyLinearEmbedding(n_components=2, n_neighbors=10)),
           ('UMAP', umap.UMAP(n_components=2, n_neighbors=15, min_dist=0.1))]

# Define the number of methods and plots per row
n_methods = 3
n_plots_per_row = n_methods

# Calculate the number of rows and columns needed
n_plots = len(methods) * len(random_states)
n_rows = int(np.ceil(n_plots / n_plots_per_row))
n_cols = n_plots_per_row

# Create the figure and axes objects
fig, axes = plt.subplots(nrows=n_rows, ncols=n_cols, figsize=(n_cols * fig_size[0], n_rows * fig_size[1]))
axes = axes.flatten()

# Loop through each method and each random state value and plot the scatter chart
for i, (method_name, method) in enumerate(methods):
    for j, random_state in enumerate(random_states):
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
        ax = axes[i * len(random_states) + j]
        ax.scatter(data_transformed[:, 0], data_transformed[:, 1], c=kmeans.labels_)
        ax.set_xlabel('Component 1')
        ax.set_ylabel('Component 2')
        ax.set_title(f'{method_name}, Random State {random_state}\nSilhouette Score: {silhouette:.2f}')
        ax.axis('square')
        
        # Adjust spacing between subplots
        fig.tight_layout()
        
        # Show the plot
        plt.show()

# Save the results to a new CSV file
results.to_csv('kmeans_results.csv', index=False)
</pre>
