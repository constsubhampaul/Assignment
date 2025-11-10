<font size="20"> **Explanation of complete process and steps followed**</font>
1. Problem Definition and Objective

The primary objective of this assignment is to determine the values of three unknown parameters—theta, M, and X—for a given set of parametric equations that define a curve. The input consists of:

The parametric equations for the curve's x and y coordinates in terms of a parameter t.

A dataset (xy_data.csv) containing x and y coordinates of points that lie on this curve.

Specified ranges for the unknown parameters and the parameter t.

The goal is to find the specific values of theta, M, and X that make the curve best fit the provided data points. The quality of this fit is measured by the L1 distance, which needs to be minimized.

2. Data Loading and Preparation

The first step was to load the provided data points from the xy_data.csv file.

The pandas library was used to read the CSV file into a DataFrame.

The x and y columns were extracted and converted into NumPy arrays, which we can call x_data and y_data.

A critical challenge was that the dataset only contained x and y values, but the parametric equations depend on the parameter t. To solve this, a t value was assigned to each data point. Since the evaluation criteria mention "uniformly sampled points," the most logical approach was to generate a sequence of uniformly spaced t values over its given range (6 < t < 60).

np.linspace(6, 60, n_points) was used to create an array of t values, where n_points is the total number of points in the dataset. This ensures that each (x, y) pair from the data has a corresponding t value.

3. Mathematical Modeling: The Error Function

To find the "best fit," we need a way to quantify how "wrong" our curve is for a given set of parameters. This is done by defining an error function (also called an objective or cost function). As per the assignment's criteria, this function is the Total L1 Distance.

The L1 distance between a predicted point (x_pred, y_pred) and an actual data point (x_data, y_data) is the sum of the absolute differences of their coordinates:
d_i = |x_pred,i - x_data,i| + |y_pred,i - y_data,i|

The total L1 error is the sum of these distances over all n data points:
Total L1 Error = sum over all i from 1 to n of ( |x_pred,i - x_data,i| + |y_pred,i - y_data,i| )

Our mathematical goal is to find the values of theta, M, and X that minimize this Total L1 Error.

4. The Optimization Process

This is an optimization problem. The scipy.optimize.minimize function from the SciPy library is perfectly suited for this task. The process was implemented as follows:

Define the Error Function in Code: A Python function (e.g., l1_error) was created. This function takes a set of parameters (theta, M, X) as input, calculates the predicted x and y values for all t_data points using the parametric equations, and returns the Total L1 Error.

Set Initial Guesses and Bounds: The minimize function requires an initial guess for the parameters. The center of the allowed ranges (e.g., theta=25, M=0, X=50) is a reasonable starting point. Crucially, the valid ranges for each parameter were provided as bounds to ensure the optimizer only searches for solutions within the specified constraints.

Run the Optimizer: The minimize function was called, passing it the error function, the initial guesses, and the bounds. The function iteratively adjusts the values of theta, M, and X, searching for the combination that produces the minimum possible L1 error.

5. Extracting and Validating the Results

Once the optimizer finished, it returned the optimal values for theta, M, and X.

Quantitative Validation: The final, minimized L1 distance was calculated to confirm the quality of the fit. A smaller value indicates a better fit.
