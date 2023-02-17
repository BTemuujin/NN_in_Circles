import numpy as np
from sklearn.neighbors import BallTree

# Load data from file
#X = np.loadtxt('positions.txt')
X = []
with open('positions.txt', 'r') as f:
    for line in f:
        x, y, z = map(float, line.strip().split())
        X.append([x, y, z])


# Set the dimensions of the box
Lx, Ly, Lz = 1,  1, 1

# Specify the range of radii to search
min_radius = 0.1
max_radius = 1
num_radii = 10
radii = np.linspace(min_radius, max_radius, num_radii)

# Create images of the positions using periodic boundary conditions
X_images = []
for i in range(-1, 2):
    for j in range(-1, 2):
        for k in range(-1, 2):
            X_images.append(X + np.array([i*Lx, j*Ly, k*Lz]))
X_images = np.concatenate(X_images)

# Create a BallTree object with the data and images
tree = BallTree(X_images)

# Create a dictionary to store the results
results = {}

# Loop over each position in the file
for i in range(len(X)):
    # Define the center point of the search circle
    center_point = X[i]

    # Loop over each radius value
    for r in radii:
        # Query the BallTree for neighbors within the specified radius
        indices = tree.query_radius([center_point], r=r)[0]

        # Store the sum of the indices of the neighbors found within the search radius in the dictionary
        key = (i, r)
        results[key] = len(indices)

# Create a 2D array of zeros to store the results
pos_results = np.zeros((len(X), len(radii)))

# Fill in the pos_results array using indexing
for key in results:
        i, r = key
        j = np.where(radii == r)[0][0]
        pos_results[i, j] = results[key]

# Print the results table
print("Position\t", end="")
for r in radii:
    print(f"r={r:.2f}\t", end="")
print()

for i in range(len(X)):
    print(f"Pos {i+1}\t", end="")
    for j in range(len(radii)):
        print(f"{int(pos_results[i,j])}\t", end="")
    print()
