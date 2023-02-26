# test2
<pre>
mixing_matrix = ica.mixing_

# Create a DataFrame to hold the results
results = pd.DataFrame(columns=['Component', 'Original Feature', 'Weight'])

# Loop through each component and original feature
for i in range(n_components):
    for j in range(data.shape[1]):
        # Compute the weight of the original feature in the current component
        weight = mixing_matrix[j, i]
        
        # Add the results to the DataFrame
        results = results.append({'Component': i+1,
                                  'Original Feature': f'Feature {j+1}',
                                  'Weight': weight},
                                 ignore_index=True)

# Print the results
print(results)
</pre>
