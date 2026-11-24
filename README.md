# Mini-Courier Planner
### Design & Analysis of Algorithms (ETCCDA302)
**K.R. Mangalam University, Gurugram, Haryana**  
**Programme:** B.Tech CSE (FSD)  
**Faculty:** Angali Pal

---

## 1. Project Overview

Mini-Courier Planner is a Python-based project that simulates the basic work of a courier delivery system. It takes a list of parcels, arranges them according to their deadlines and priorities, selects parcels that fit inside a delivery vehicle, calculates shortest distances between locations, and prepares an approximate delivery route.

I developed this project to understand how the algorithms covered in the DAA course can be used together to solve a practical problem.

## 2. Problem Statement

A courier service has different parcels. Every parcel has a delivery location, weight, value, deadline, and priority. The delivery vehicle has a fixed weight capacity, and the delivery locations are connected by roads with different distances.

The system should:
- Arrange parcels according to their deadlines and priorities.
- Select a valuable combination of parcels without exceeding the vehicle capacity.
- Calculate shortest paths from the warehouse to delivery locations.
- Prepare an approximate route that visits the selected delivery locations and returns to the warehouse.
- Test the algorithms with different input sizes and compare their execution times.

## 3. Project Objectives

- Implement and compare Merge Sort and Quick Sort.
- Use 0/1 Knapsack Dynamic Programming for parcel selection.
- Represent delivery locations as a weighted graph.
- Implement Dijkstra's Algorithm for shortest-path calculation.
- Use the Nearest Neighbor method for an approximate TSP delivery route.
- Test the algorithms and record their execution time.
- Present the output, performance graphs, and project results.

## 4. Technologies Used

- **Programming language:** Python
- **Data format:** JSON
- **Testing:** Python test scripts
- **Performance analysis:** CSV files and graphs
- **Development environment:** Visual Studio Code

## 5. Project Folder Structure

```text
Mini-Courier-Planner/
│
├── algorithms/
│   ├── __init__.py
│   ├── sorting.py
│   ├── knapsack.py
│   ├── dijkstra.py
│   └── tsp.py
│
├── data/
│   └── locations.json
│
├── tests/
│   ├── __init__.py
│   ├── test_algorithms.py
│   ├── test_performance.py
│   ├── test_dp_graph_performance.py
│   ├── plot_performance.py
│   └── plot_other_performance.py
│
├── results/
│   ├── sorting_performance.csv
│   ├── other_algorithm_performance.csv
│   ├── sorting_comparison.png
│   ├── knapsack_performance.png
│   └── dijkstra_performance.png
│
├── screenshots/
├── main.py
├── config.py
├── requirements.txt
└── README.md
```

The `algorithms` folder contains the algorithm implementations. The `data` folder stores the delivery graph. The `tests` folder contains correctness tests, performance tests, and graph-generation scripts. The `results` folder stores measured data and generated charts. The `screenshots` folder is for evidence captured while running the project.

---

# Phase 1: Courier Problem Design and Planning

## 6. Understanding the Problem

The project is designed around a small courier service. The warehouse is the starting point, and customers are represented by different delivery locations.

Each parcel contains the following information:

| Attribute | Meaning |
|---|---|
| Parcel ID | Unique parcel name, such as P101 |
| Destination | Delivery location |
| Weight | Parcel weight in kg |
| Value | Value assigned to the parcel |
| Deadline | Expected delivery deadline |
| Priority | Importance of the parcel |

The vehicle capacity used in the main example is **20 kg**.

## 7. Sample Parcel Data

| Parcel | Destination | Weight | Value | Deadline | Priority |
|---|---:|---:|---:|---:|---:|
| P101 | A | 5 kg | 10 | 14:00 | 5 |
| P102 | B | 8 kg | 15 | 10:00 | 9 |
| P103 | C | 6 kg | 12 | 12:00 | 8 |
| P104 | D | 4 kg | 8 | 10:00 | 7 |

## 8. Delivery Graph Design

The delivery network is represented as a weighted graph using an adjacency-list structure in `data/locations.json`.

- **W** represents the warehouse.
- **A, B, C, and D** represent customer locations.
- The edge weights represent road distances in kilometres.

| Location | Connected locations and distances |
|---|---|
| W | A: 10 km, B: 15 km, C: 20 km, D: 25 km |
| A | W: 10 km, B: 5 km, C: 12 km |
| B | W: 15 km, A: 5 km, C: 8 km, D: 10 km |
| C | W: 20 km, A: 12 km, B: 8 km, D: 6 km |
| D | W: 25 km, B: 10 km, C: 6 km |

## 9. Algorithms Selected

| Algorithm | Purpose |
|---|---|
| Merge Sort | Sort parcels by deadline and priority |
| Quick Sort | Sort parcels using the same conditions and compare performance |
| 0/1 Knapsack | Select parcels within the vehicle's weight capacity |
| Dijkstra's Algorithm | Find shortest distances from the warehouse |
| Nearest Neighbor TSP | Build an approximate route through delivery locations |

## 10. Overall Working Plan

1. Read the parcel details and delivery graph.
2. Sort the parcels by deadline and priority.
3. Use Knapsack Dynamic Programming to select parcels within the vehicle capacity.
4. Run Dijkstra's Algorithm from the warehouse to calculate shortest-path distances.
5. Apply the Nearest Neighbor method to create a delivery route for the selected destinations.
6. Display the parcel list, packing result, shortest distances, and route.

---

# Phase 2: Package Prioritization and Box Filling

## 11. Parcel Sorting

The project uses two sorting algorithms:

### 11.1 Merge Sort

Merge Sort divides the parcel list into smaller parts, sorts those parts, and merges them into one sorted list.

The sorting condition is:
- Parcels with earlier deadlines come first.
- If two parcels have the same deadline, the parcel with higher priority comes first.

**Time complexity:** O(n log n)  
**Auxiliary space complexity:** O(n)

### 11.2 Quick Sort

Quick Sort selects a pivot and partitions the list around it. It uses the same deadline and priority conditions as Merge Sort.

**Average time complexity:** O(n log n)  
**Worst-case time complexity:** O(n²)

### 11.3 Sorting Result

For the sample parcels, the sorted order is:

| Order | Parcel | Deadline | Priority |
|---:|---|---|---:|
| 1 | P102 | 10:00 | 9 |
| 2 | P104 | 10:00 | 7 |
| 3 | P103 | 12:00 | 8 |
| 4 | P101 | 14:00 | 5 |

P102 comes before P104 because both have the same deadline, but P102 has a higher priority.

## 12. 0/1 Knapsack Dynamic Programming

The vehicle has a maximum capacity of 20 kg. Every parcel has a weight and a value.

The 0/1 Knapsack algorithm decides whether to include or exclude each parcel. A parcel cannot be selected partially. Dynamic Programming is used to find the combination with the highest total value without crossing the vehicle capacity.

**Time complexity:** O(nW)  
**Space complexity:** O(nW) for the DP table

Here, `n` is the number of parcels and `W` is the vehicle capacity.

### 12.1 Packing Result

The selected parcels are:

| Parcel | Weight | Value |
|---|---:|---:|
| P102 | 8 kg | 15 |
| P103 | 6 kg | 12 |
| P101 | 5 kg | 10 |
| **Total** | **19 kg** | **37** |

- Vehicle capacity: 20 kg
- Selected weight: 19 kg
- Total selected value: 37
- Remaining capacity: 1 kg

The algorithm selects P102, P103, and P101. P104 is not selected in this result.

---

# Phase 3: Route Maker and Graph Processing

## 13. Graph Representation

The delivery locations are stored as a weighted graph. Each location is a vertex, and each road connection is an edge. The distance of a road is stored as its weight.

The graph is stored in `data/locations.json` and is used by the shortest-path and route-planning algorithms.

## 14. Dijkstra's Algorithm

Dijkstra's Algorithm finds the shortest distance from a starting vertex to all other reachable vertices in a graph with non-negative edge weights.

In this project:
- The starting point is warehouse **W**.
- The algorithm calculates the shortest distance from W to each delivery location.
- A min-heap priority queue is used to process the nearest available location efficiently.

**Time complexity with a min-heap:** O((V + E) log V)

Here, `V` is the number of vertices and `E` is the number of edges.

### 14.1 Shortest-Path Result

| Destination | Shortest distance from W |
|---|---:|
| W | 0 km |
| A | 10 km |
| B | 15 km |
| C | 20 km |
| D | 25 km |

These are shortest-path distances from the warehouse. They are not the total distance of the complete delivery tour.

## 15. Nearest Neighbor TSP Approximation

The Travelling Salesperson Problem (TSP) asks for a route that visits the required locations and returns to the starting point.

This project uses the **Nearest Neighbor** approximation method:
1. Start from the warehouse.
2. Select the nearest unvisited delivery location.
3. Mark that location as visited.
4. Repeat until all required locations have been visited.
5. Return to the warehouse.
6. Add the travelled distances to calculate the route length.

**Time complexity:** O(k²), where `k` is the number of delivery locations.

Nearest Neighbor is an approximation technique. It is simple and useful for this project, but it does not guarantee the globally shortest possible route.

### 15.1 Main Program Route

The Knapsack result selects destinations A, B, and C. For these selected destinations, the main program produces:

```text
W -> A -> B -> C -> W
Total Route Distance: 43 km
```

### 15.2 Separate TSP Test

The separate TSP test uses all four customer locations A, B, C, and D. Its output is:

```text
W -> A -> B -> C -> D -> W
Total Route Distance: 54 km
```

These route lengths are different because the tests use different sets of delivery locations.

---

# Phase 4: Performance Testing and Optimization

## 16. Performance Testing Method

The algorithms were tested with different input sizes. The execution time was recorded in seconds so that the effect of increasing input size could be observed.

The performance data is stored in CSV files in the `results` folder. The project also generates graphs for sorting, Knapsack, and Dijkstra.

## 17. Sorting Performance

Merge Sort and Quick Sort were tested with 100, 500, 1,000, and 2,000 parcels.

| Number of parcels | Merge Sort (seconds) | Quick Sort (seconds) |
|---:|---:|---:|
| 100 | 0.000346 | 0.000192 |
| 500 | 0.003618 | 0.001834 |
| 1,000 | 0.004771 | 0.004707 |
| 2,000 | 0.015599 | 0.008890 |

**Observation:** In these test runs, Quick Sort took less time than Merge Sort for the tested input sizes. The exact timings can change depending on the input data and computer.

<sub>Performance chart: `results/sorting_comparison.png`</sub>

## 18. Knapsack Performance

Knapsack was tested with different numbers of parcels while keeping the vehicle capacity fixed at 500 kg.

| Number of parcels | Capacity | Execution time (seconds) |
|---:|---:|---:|
| 20 | 500 kg | 0.002641 |
| 50 | 500 kg | 0.008052 |
| 100 | 500 kg | 0.013946 |
| 200 | 500 kg | 0.031251 |

**Observation:** The execution time increased as the number of parcels increased. This is expected because the number of DP states depends on both the number of parcels and the capacity.

<sub>Performance chart: `results/knapsack_performance.png`</sub>

## 19. Dijkstra Performance

Dijkstra's Algorithm was tested with graphs containing different numbers of locations.

| Number of locations | Execution time (seconds) |
|---:|---:|
| 50 | 0.000230 |
| 100 | 0.000253 |
| 200 | 0.000793 |
| 400 | 0.001750 |

**Observation:** The measured execution time increased as the graph size increased.

<sub>Performance chart: `results/dijkstra_performance.png`</sub>

## 20. Time Complexity Summary

| Algorithm | Time complexity |
|---|---|
| Merge Sort | O(n log n) |
| Quick Sort | Average O(n log n), worst O(n²) |
| 0/1 Knapsack DP | O(nW) |
| Dijkstra with min-heap | O((V + E) log V) |
| Nearest Neighbor TSP | O(k²) |

Where:
- `n` = number of parcels
- `W` = vehicle capacity
- `V` = number of graph vertices
- `E` = number of graph edges
- `k` = number of delivery locations

## 21. Possible Improvements

- Reuse shortest-path results when the delivery graph has not changed.
- Use a one-dimensional DP array to reduce Knapsack memory usage.
- Test with more parcel counts, vehicle capacities, and graph sizes.
- Try different parcel data and delivery graphs to check the program with new inputs.
- Compare the route produced by Nearest Neighbor with other route-planning approaches in future work.

---

# Phase 5: Final Report and Demonstration

## 22. Testing and Verification

The project includes test scripts to check the correctness of the algorithms and measure their performance.

Run the correctness tests from the project root:

```powershell
python -m tests.test_algorithms
```

The tests check the sorting algorithms, Knapsack result, and TSP route. The main integrated program can be run with:

```powershell
python main.py
```

Performance tests and graph-generation scripts:

```powershell
python -m tests.test_performance
python -m tests.test_dp_graph_performance
python -m tests.plot_performance
python -m tests.plot_other_performance
```

Run these commands from the `Mini-Courier-Planner` project folder. The `-m` option runs the test files as Python modules from the project root.

## 23. Main Program Output

The main program brings the different parts together. Its sample output is:

```text
PARCEL SORTING:
P102 | Deadline: 10:00 | Priority: 9
P104 | Deadline: 10:00 | Priority: 7
P103 | Deadline: 12:00 | Priority: 8
P101 | Deadline: 14:00 | Priority: 5

VEHICLE PACKING:
P102 destination B weight 8 value 15
P103 destination C weight 6 value 12
P101 destination A weight 5 value 10

Total Weight: 19 kg
Total Value: 37
Remaining Capacity: 1 kg

DIJKSTRA:
W -> W: 0 km
W -> A: 10 km
W -> B: 15 km
W -> C: 20 km
W -> D: 25 km

TSP:
W -> A -> B -> C -> W
Total Route Distance: 43 km
```

## 24. Screenshots and Evidence

Add genuine screenshots captured from your own VS Code terminal or project. The following are useful evidence for the assignment:

| Suggested screenshot | What it should show |
|---|---|
| `phase1_design.png` | Project design, parcel information, and graph structure |
| `phase2_sorting_knapsack.png` | Sorting and Knapsack test output |
| `phase3_dijkstra_tsp.png` | Shortest-path and TSP output |
| `phase5_main_program.png` | Complete output of `python main.py` |
| `phase4_sorting_graph.png` | Sorting performance chart |
| `phase4_knapsack_graph.png` | Knapsack performance chart |
| `phase4_dijkstra_graph.png` | Dijkstra performance chart |
| `phase4_performance_results.png` | Performance test output or CSV results |

The chart images are already stored in the `results` folder. You can also insert them directly into this README using relative image links if required, for example:

```markdown
![Sorting Performance](results/sorting_comparison.png)
![Knapsack Performance](results/knapsack_performance.png)
![Dijkstra Performance](results/dijkstra_performance.png)
```

Only include screenshots that you have actually captured. Do not use placeholder screenshots as evidence.

## 25. Installation and Execution

### Step 1: Open the project folder

Open the `Mini-Courier-Planner` folder in Visual Studio Code.

### Step 2: Create a virtual environment (recommended)

```powershell
python -m venv .venv
```

### Step 3: Activate the virtual environment

```powershell
.venv\Scripts\Activate.ps1
```

If PowerShell blocks activation, use the Python executable inside the environment directly or adjust the execution policy according to your system settings.

### Step 4: Install dependencies

```powershell
python -m pip install -r requirements.txt
```

### Step 5: Run the main program

```powershell
python main.py
```

### Step 6: Run the tests

```powershell
python -m tests.test_algorithms
```

### Step 7: Run performance tests and generate charts

```powershell
python -m tests.test_performance
python -m tests.test_dp_graph_performance
python -m tests.plot_performance
python -m tests.plot_other_performance
```

## 26. Conclusion

I built the Mini-Courier Planner by combining sorting, Dynamic Programming, and graph algorithms in one Python project. It sorts parcels according to their deadlines and priorities, selects parcels that fit inside the vehicle, calculates shortest distances, and prepares an approximate delivery route.

After running the tests and performance scripts, I could check whether the algorithms were giving the expected results and see how their execution time changed with larger inputs. This project helped me understand how the algorithms studied in the DAA course can be connected and used to solve a practical problem.

## 27. Submission Checklist

Before submitting the project, check that the folder contains:

- [ ] Python source code and `main.py`
- [ ] Sample delivery graph in `data/locations.json`
- [ ] Test scripts in the `tests` folder
- [ ] Performance CSV files and generated charts in `results`
- [ ] Actual screenshots in the `screenshots` folder
- [ ] This complete `README.md`
- [ ] `requirements.txt`

**Note:** Do not include the `.venv` folder in the final ZIP or GitHub submission. It can be recreated using the installation steps above.
