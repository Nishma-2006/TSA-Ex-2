# TSA-Ex-2
# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date:
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Read dataset
data = pd.read_csv('/content/ice_cream.csv')

data.head()

# Convert DATE column and set index
data['DATE'] = pd.to_datetime(data['DATE'])
data.set_index('DATE', inplace=True)

# Resample monthly data into yearly data
resampled_data = data['IPN31152N'].resample('YE').sum().to_frame()

# Extract year
resampled_data.index = resampled_data.index.year
resampled_data.reset_index(inplace=True)
resampled_data.rename(columns={'DATE': 'Year'}, inplace=True)

resampled_data.head()

# Get years and ice cream values
years = resampled_data['Year'].tolist()
ice_cream = resampled_data['IPN31152N'].tolist()

# Linear Trend Estimation
X = [i - years[len(years) // 2] for i in years]
x2 = [i ** 2 for i in X]
xy = [i * j for i, j in zip(X, ice_cream)]

n = len(years)

b = (n * sum(xy) - sum(ice_cream) * sum(X)) / (
    n * sum(x2) - sum(X) ** 2
)

a = (sum(ice_cream) - b * sum(X)) / n

linear_trend = [a + b * X[i] for i in range(n)]

# Polynomial Trend Estimation (Degree 2)
x3 = [i ** 3 for i in X]
x4 = [i ** 4 for i in X]
x2y = [i * j for i, j in zip(x2, ice_cream)]

coeff = [
    [n, sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [sum(ice_cream), sum(xy), sum(x2y)]

A = np.array(coeff)
B = np.array(Y)

solution = np.linalg.solve(A, B)
a_poly, b_poly, c_poly = solution

poly_trend = [
    a_poly + b_poly * X[i] + c_poly * X[i] ** 2
    for i in range(n)
]

# Display equations
print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")

print(
    f"Polynomial Trend: y = {a_poly:.2f} "
    f"+ {b_poly:.2f}x + {c_poly:.2f}x²"
)

# Add trends to dataset
resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend

# Set Year as index
resampled_data.set_index('Year', inplace=True)

# Plot results
resampled_data['IPN31152N'].plot(
    kind='line', color='blue', marker='o',
    label='Original Data'
)

resampled_data['Linear Trend'].plot(
    kind='line', color='black', linestyle='--',
    label='Linear Trend'
)

resampled_data['Polynomial Trend'].plot(
    kind='line', color='red', marker='o',
    label='Polynomial Trend'
)

plt.title('Linear and Polynomial Trend Estimation - Ice Cream Data')
plt.xlabel('Year')
plt.ylabel('IPN31152N')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()
```

### OUTPUT

<img width="509" height="309" alt="image" src="https://github.com/user-attachments/assets/81fa9658-d129-4712-8c5e-c844a1438128" />


### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
