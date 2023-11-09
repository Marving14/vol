# Time to Collision Estimation Function Documentation

## Objective
The objective of this function is to estimate the time to collision (TTC) between an ego vehicle and a vehicle in front of it, considering both vehicles' velocities and accelerations.

## Problem Statement
Given the initial velocities and accelerations of both the ego vehicle and the front vehicle, along with the initial distance between them, the function calculates the time at which the distance between the two vehicles will be zero, indicating a potential collision.

## Approach
The solution involves calculating the relative motion between the two vehicles. This is achieved by determining the relative velocity and acceleration and using these to predict when the vehicles might occupy the same position.

### Relative Motion Equations
Relative velocity is the difference in velocity between the ego vehicle and the front vehicle. Relative acceleration is the difference in their accelerations.

The motion equation is:
\[ s = v_0 t + \frac{1}{2} a t^2 \]

Where:
- \( s \) is the distance between the vehicles,
- \( v_0 \) is the relative initial velocity,
- \( a \) is the relative acceleration,
- \( t \) is the time.

### Solving for Time
The time \( t \) when the distance \( s \) becomes zero is found by solving the quadratic equation:
\[ \frac{1}{2} a t^2 + v_0 t - s = 0 \]

Using the quadratic formula:
\[ t = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a} \]

### Handling Different Scenarios
- If the discriminant (\( b^2 - 4ac \)) is negative, the vehicles will not collide as they are moving apart.
- If the discriminant is zero, there is one point of contact, and the time to collision is calculated accordingly.
- If the discriminant is positive, the smallest positive root is the time to collision.
- If the calculated time to collision is negative, it indicates that the vehicles are moving apart, and no collision will occur.

## Implementation
The `calculate_ttc_with_acceleration` function in Python handles the TTC calculation. It returns the time to collision if a collision is possible, or a message indicating that no collision is possible otherwise.

### Test Cases
The function is tested with various scenarios, including:
- Basic cases with both vehicles accelerating.
- Cases where the ego vehicle is catching up to a decelerating front vehicle.
- Scenarios where vehicles are moving apart, and no collision is possible.

The results from these test cases confirm the function's accuracy and reliability.



# Vehicle Position Visualization Documentation

## Overview
Overview of the Python script used to visualize vehicle positions and their bounding boxes in a bird's eye view. The script reads vehicle detection data from a CSV file and calculates bounding boxes based on each vehicle's heading, width, and length. These bounding boxes are then visualized on a plot to represent the vehicles' positions on the road.

## Data Source
The vehicle position data is sourced from a CSV file named "vehicle_data.csv". The file contains the following columns:
- `Timestamp`: The time at which the vehicle's position was recorded.
- `Vehicle_ID`: A unique identifier for each vehicle.
- `Lateral_Position`: The lateral position of the vehicle on the road.
- `Longitudinal_Position`: The longitudinal position of the vehicle on the road.
- `Heading`: The orientation of the vehicle in degrees.
- `Width`: The width of the vehicle.
- `Length`: The length of the vehicle.

## Calculation of Bounding Boxes
Bounding boxes are calculated using the `calculate_bounding_box` function, which takes the vehicle's position, size, and heading to compute the corners of the bounding box. The corners are then rotated based on the vehicle's heading to align with the vehicle's orientation.

## Visualization
The script generates a plot with each vehicle represented by a bounding box. The plot is scaled to include all vehicles in the dataset and is set to an equal aspect ratio to maintain the correct proportions. The vehicles are plotted with a blue outline by default.

## Usage
To run the script, ensure that the CSV file is located in the specified directory and execute the script. The resulting plot will be displayed, and an image file named "vehicle_positions.png" will be saved with a resolution of 300 DPI.

## The computation of bounding boxes for each vehicle is based on the vehicle's position, size, and orientation data provided in the CSV file. Here's a step-by-step explanation of the process:

### Computing Bounding Boxes
- `Center Position:` The Lateral_Position and Longitudinal_Position fields from the dataset provide the center point of each vehicle.

- `Vehicle Dimensions:` The Width and Length fields define the size of the vehicle.

- `Orientation:` The Heading field gives the orientation of the vehicle in degrees, which is the angle from the longitudinal axis of the road.

- `Rectangle Corners:` Before rotation, the corners of the bounding box are calculated relative to the center position, assuming the vehicle is aligned with the axes.

- `Rotation:` The corners are then rotated around the center point by the heading angle to align with the vehicle's actual orientation. This is done using standard 2D rotation matrix transformations.

- `Rotation Matrix:` The rotation matrix is defined as:

    [ cos(θ)   - sin(θ)  ] 
   \
    [ sin(θ)    cos(θ)    ]

where 
�
θ is the heading angle converted to radians.

- Applying Rotation: Each corner point is multiplied by the rotation matrix to get the rotated coordinates.

- Storing Bounding Boxes: The rotated corners are stored as a list of tuples representing the bounding box for each vehicle.
### Why This Approach?
This approach is chosen for its mathematical precision in representing the vehicles' positions and orientations. The use of rotation matrices is a standard technique for handling rotations in two dimensions, which is crucial for accurately depicting the vehicles' headings. 



# Sensor Data vs Reference Data Analysis
## Objective
My goal in this analysis was to compare the sensor data from multiple vehicles against a set of reference data to determine which vehicle's sensor readings most closely matched the reference and to quantify the sensor's accuracy.

## Methodology
I approached the problem methodically:

- `Data Loading:` I began by loading the sensor and reference data from their respective CSV files into Pandas DataFrames for analysis.

- `Initial Visualization:` I plotted the lateral and longitudinal positions of all the vehicles alongside the reference data to visually determine which vehicle's path was most similar to the reference.

- `Identification of Closest Vehicle:` From the plots, it became clear that Vehicle 2's trajectory was the closest match to the reference data.

- `Data Alignment:` To ensure an accurate comparison, I aligned the timestamps of Vehicle 2's data with those of the reference data.

- `Error Calculation:` I calculated the lateral and longitudinal errors between Vehicle 2's sensor data and the reference data to measure the discrepancies.

- `Error Analysis:` I computed the Mean Squared Error (MSE) and Mean Absolute Error (MAE) for a numerical representation of the sensor's accuracy.

- `Final Visualization:` I created additional plots to visualize the errors over time, which provided a clearer understanding of the sensor's performance.

## Code Adjustments
The code was refined through several iterations:

- `Filtering:` After identifying Vehicle 2 as the vehicle of interest, I filtered the dataset to include only its data.
- `Error Computation:` I introduced functions to calculate both lateral and longitudinal errors.
- `Statistical Summary:` I generated summary statistics to provide a more nuanced view of the error distribution.

## Results
The analysis yielded the following insights:

- `Lateral Errors:` The lateral position errors for Vehicle 2 had a mean of approximately 1.391, with a relatively low standard deviation, indicating consistent lateral positioning by the sensor.
- `Longitudinal Errors:` The longitudinal position errors showed greater variability, with a mean error of approximately -1.632.

## Conclusion
I concluded that Vehicle 2's sensor data was the closest to the reference data. The errors I calculated revealed that the sensor's lateral measurements were more reliable than the longitudinal ones. This analysis will be instrumental in guiding further sensor calibration and improving data processing techniques.