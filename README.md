# Quantum Bin Packing for Vehicle Routing

## Product overview

This Solution suggests optimal allocation of marketing budget across channels like print media, social media, radio, television, etc. based on their past performance using bayesian based market budget allocation method. Alongside, the solution accounts for the effect of non-marketing attributes- such as timeseries and contextual factors- on sales/revenue while recommending optimal budget allocation. The solution compares pre and post budget allocation of each marketing channel and computes predicted target KPI (revenue/ sales).

## Product Highlight 

* This solution helps organizations measure advertising effectiveness and suggest budget allocation across marketing channels to maximize the return on investment. This inturn helps to recommend optimal budget allocation for each media channel. The solution accounts the impact of time series factors such as trend and seasonality of sales while performing budget optimisation.
* The solution understands the effect of advertising efforts as two components: First, Carryover effect that accounts for delayed consumer response. Second, Diminishing return effect to account for the saturation of advertizing spend on a media channel.  The output chart compares the revenue impact of budget allocation: Average past spendings in each channel as current budget allocation Vs. Optimised Budget allocation.
* Mphasis HyperGraf is an omni-channel customer 360 analytics solution. Need customized Deep Learning/NLP solutions? Get in touch!

## Amazon Marketplace Link
The product can be found [here]([https://aws.amazon.com/marketplace/pp/prodview-l5mfrplslsg5s])
  

## Description

This repository provides a quantum-based solution for efficient bin packing specifically tailored for vehicle routing applications. Our solution maximizes space utilization while enabling sequential unloading aligned with delivery routes, ensuring rapid retrieval of packages.

**Note:** For the D-Wave solver to provide valid solutions, the cumulative volume of packed boxes should not exceed **85-90%** of the bin's total volume.

## Input File Format

### `data.csv`

This file includes package dimensions with the following mandatory columns:

- `id`: Unique identifier for each box type.
- `quantity`: Number of boxes of each type (mandatory for any bin packing).
- `length`, `width`, `height`: Dimensions of each box.

Optional columns (recommended for advanced packing strategies):

- `weight`: Utilized for load-bearing considerations and optimizing the packing according to user-specified center of mass.
- `Ordering`: Specifies the unloading sequence of boxes according to delivery routes.

**Example:**

| id | quantity | length | width | height | weight | Ordering          |
| -- | -------- | ------ | ----- | ------ | ------ | ----------------- |
| 0  | 9        | 114    | 207   | 181    | 20     | 4,3,1,3,3,4,1,2,4 |
| 1  | 6        | 158    | 220   | 101    | 20     | 3,2,1,2,3,2       |
| 2  | 7        | 206    | 270   | 199    | 20     | 2,1,1,3,3,1,3     |

---

### `token.json`

Contains authentication tokens and solver configuration:

```json
{
  "token": "XYXYXYXYXYXYXYX",
  "weightage_obj_1": 1.0,
  "solver_seconds": 30
}
```

- **token**: D-Wave authentication token.
- **weightage\_obj\_1**: Objective weightage for optimization i.e. increase the weightage value for removing floating boxes
- **solver\_seconds**: Maximum allowed runtime for the solver.

---

### `input_variables.json`

Specifies bin dimensions and constraints:

```json
{
  "load_bearing_ratio": null,
  "bin_dimensions": {
    "length": 1500,
    "width": 1500,
    "height": 1500
  },
  "center_of_mass": null
}
```

- **bin\_dimensions**: Length, width, and height of the bin.
- **load\_bearing\_ratio** *(optional)*: Ratio of heavier to lighter boxes, specifying load-bearing capacity.
- **center\_of\_mass** *(optional)*: User-specified coordinates for desired center of mass (COM) location on the XY plane.

---

## Output Format

The solver generates a structured text file detailing the optimized packing arrangement. Each row corresponds to a box placed in the bin, specifying the following variables:

| Column          | Description                                                   |
|-----------------|---------------------------------------------------------------|
| **id**          | Unique identifier of the box type as specified in `data.csv`. |
| **bin-location**| Identifier for the bin used (useful when multiple bins are involved). |
| **orientation** | Numerical indicator of box orientation (rotation alignment).  |
| **x, y, z**     | Coordinates of the box position within the bin (origin-based at the bin corner).|
| **x', y', z'**  | Dimensions of the box after orientation, aligned along x, y, z axes respectively.|
| **weight**      | Weight of the box as specified in the input file.             |

Additional summary information provided at the top of the output file includes:

- **Number of bins used**: Total bins utilized in the solution.
- **Number of cases packed**: Total number of boxes successfully packed.
- **Bin dimensions (L * W * H)**: Dimensions of the bin utilized for packing.
- **Objective value**: Numerical evaluation of solution quality based on optimization criteria (e.g., space efficiency, stability).

### Example Output

```
# Number of bins used: 1
# Number of cases packed: 51
# Bin dimensions (L * W * H): 1500 1500 1500
# Objective value: 1548.882

  id    bin-location    orientation     x     y    z    x'    y'    z'    weight
----  --------------  -------------  ----  ----  ---  ----  ----  ----  --------
   0               1              6    30   600    0   181   207   114        20
   0               1              6   517     0    0   181   207   114        20
   0               1              6     0     0  101   181   207   114        20
   0               1              4     0   406  114   207   181   114        20
```


---

## Usage

Place your input files (`data.csv`, `token.json`, and `input_variables.json`) in the designated folder and run the solver to generate optimized packing arrangements.

## Dependencies

- D-Wave Solver token

