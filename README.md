# OR Solution: README

## Overview

This proof of concept involves solving an inventory and distribution optimization problem using linear programming and routing techniques. The problem combines aspects of demand forecasting, inventory management, and distribution logistics, including a focus on the Traveling Salesman Problem (TSP) for efficient routing.

## Procedure

### 1. Data Loading and Preprocessing

- Input data includes:
  - `Outlet Details`: Provides information about outlets and their locations.
  - `Sales Data`: Historical sales data, used to forecast demand.
  - `Vehicle Details`: Specifies available vehicles and their capacities.
  - `Inventory Details`: Per unit inventory cost.
- Data is read using `pandas` and cleaned/preprocessed to ensure consistency.

### 2. Demand Forecasting

- Grouped historical sales data by outlet and product to calculate maximum demand.
- By using the maximum demand for each outlet and product, the approach minimizes the risk of under-distributing inventory, ensuring that demand is met even under peak conditions.

### 3. Distance Calculation

- Used the Haversine formula to calculate straight-line distances between locations.
- In an optimal solution, actual route distances should be used for more accurate results.
- As a future enhancement, a routing API such as Google Maps Distance Matrix API can be integrated to obtain real-world distances.

### 4. Priority Measures

- Each outlet is assigned a priority measure. In this proof of concept, the priority measure is calculated using a randomly assigned value.

- Other possible ways to assign priority could include:
    - Demand Volume: Outlets with higher demand for certain products might be prioritized for supply or delivery to ensure that high-demand outlets receive their required stock promptly

    - Revenue Potential: Outlets with higher revenue potential could be given higher priority.

    - Distance from Distribution Centre: Outlets closer to the distribution centre may have a different priority to optimize logistics.

### 5. Distribution Center Assumption
In this Proof of Concept (POC), the location used as the distribution center is not an actual facility but a selected point near a potential distribution area.
For a real-world implementation, determining the exact position of a distribution center is crucial. A gravitational model approach can be applied to calculate an optimal location based on factors such as demand distribution and transportation costs.
This enhancement would allow the solution to effectively establish a distribution center that minimizes costs and improves overall efficiency.
### 6. Linear Programming Approach

- Defined decision variables for allocating inventory to outlets (`q[i, j]`) and routing vehicles (`y[j]`).
- Modeled constraints:
  - **Vehicle Capacity Constraint**: For each outlet, the total quantity distributed (`q`) must not exceed the combined capacity of the vehicles serving that outlet.
  
  - **Demand Satisfaction Constraint**: Ensures that the quantity delivered for each product at a store does not exceed the store’s demand for that product.
- Objective Function:
  - **Revenue**: Based on the allocated quantity (`q[i, j]`) multiplied by  priority (`Pr[j]`) and Adjusted price (`A[i]`).
  - **Delivery Costs**: Accounts for transportation cost (`C[j] * y[j]`).
  - **Inventory Costs**: Accounts for inventory holding costs (`IC[i] * q[i, j]`).
  - **Optimization Goal**: Maximize the net profit (`Z`), calculated as `Revenue - Delivery Costs - Inventory Costs`.

### 7. Traveling Salesman Problem (TSP)
- Calculated for outlets of a specific beat.
- For routing optimization, formulated a TSP variant:
  - Minimize the total travel distance for vehicle routes.
  - Ensured valid routes with no sub-tours and coverage of all outlets.

### 8. Solver: CPLEX Enterprise Edition
- Note: The CPLEX Community Edition imposes a limit on the number of constraints, necessitating the use of the Enterprise Edition.

## Dependencies

The following dependencies are required to run this project:

```
pandas
numpy
openpyxl
docplex
folium
plotly
dash
nbformat>=4.2.0
```

## How to Run

1. Install the dependencies:
   ```
   pip install -r requirements.txt
   # install enterprise edition python client
   python /Applications/CPLEX_Studio2211/python/setup.py install
   ```
2. Ensure IBM CPLEX is installed and configured on your system.
3. Open the Jupyter Notebook and execute the cells in order.

## Outputs

- The optimized solution includes:
  - Allocation of inventory to outlets
  - Vehicle routes

## Key Notes

- The project relies on linear programming and routing techniques to address real-world constraints.
- Due to the complexity of the problem, the use of the CPLEX Enterprise Edition is essential for solving large-scale models without constraint limitations.
