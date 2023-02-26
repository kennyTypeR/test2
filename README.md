# test2
<pre>
# Step 1: Use FastICA to transform the data
n_components = 5 # Specify the number of independent components to extract
ica = FastICA(n_components=n_components, random_state=42)
ica.fit(X)
X_ica = ica.transform(X)

# Step 2: Get the mixing matrix and transpose it
mixing_matrix = ica.mixing_
mixing_matrix_t = mixing_matrix.T

# Step 3: Create an empty list to store the rows of the dataframe
rows = []

# Step 4: Loop over the independent components
for component_idx in range(n_components):
    
    # Step 5: Create a list of tuples for the feature weights in the component
    feature_weights = []
    for feature_idx in range(X.shape[1]):
        weight = mixing_matrix_t[component_idx, feature_idx]
        feature_name = f'Feature {feature_idx}' # Replace with actual feature names
        feature_weights.append((feature_name, weight))
    
    # Step 6: Append a new row to the list for each tuple
    for feature_name, weight in feature_weights:
        rows.append((component_idx, feature_name, weight))

# Step 7: Create a dataframe from the list of rows
df = pd.DataFrame(rows, columns=['Component', 'Feature', 'Weight'])
</pre>
