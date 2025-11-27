# hhcrsp

Data and Toolbox Repository for the Home Healthcare Routing and Scheduling Problem.

This repository contains the instances, some selected solutions, and a toolbox for the generation of instances and the validation 
presented in the paper *Multi-Neighborhood Simulated Annealing for the Home Healthcare Routing and Scheduling Problem* by Sara Ceschia, Luca Di Gaspero, Roberto Maria Rosati, and Andrea Schaerf, *International Transactions in Operational Research*, 2024, [https://doi.org/10.1111/itor.13585](https://doi.org/10.1111/itor.13585).

A new version of the validation and generation toolbox, including an interactive solution visualizer, is currently under development and is available at [https://github.com/iolab-uniud/hhcrsp-toolbox](https://github.com/iolab-uniud/hhcrsp-toolbox).

------------------------------------------------------------------------

## Data

### Instance format

The instance format is JSON with the following structure:

``` json
{
  "patients": [
    {
      "id": "p1",
      "location": [45.62, 13.21],
      "time_window": [
        240,
        360
      ],
      "required_caregivers": [
        {
          "service": "s2",
          "duration": 30
        }
      ]
    },
    {
      "id": "p2",
      "time_window": [
        120,
        180
      ],
      "required_caregivers": [
        {
          "service": "s3",
          "duration": 20
        }
      ]
    },
    {
      "id": "p3",
      "time_window": [
        0,
        60
      ],
      "required_caregivers": [
        {
          "service": "s2",
          "duration": 45
        }
      ]
    },
    {
      "id": "p4",
      "time_window": [
        120,
        210
      ],
      "required_caregivers": [
        {
          "service": "s2",
          "duration": 30
        },
        {
          "service": "s3",
          "duration": 30
        }
      ],
      "synchronization": {
        "type": "simultaneous"
      }
    },
    {
      "id": "p5",
      "time_window": [
        270,
        420
      ],
      "required_caregivers": [
        {
          "service": "s1",
          "duration": 15
        },
        {
          "service": "s3",
          "duration": 30
        }
      ],
      "synchronization": {
        "type": "sequential",
        "distance": [
          30,
          45
        ]
      }
    },
    {
      "id": "p6",
      "time_window": [
        360,
        420
      ],
      "required_caregivers": [
        {
          "service": "s1",
          "duration": 45
        },
        {
          "service": "s3",
          "duration": 20
        }
      ],
      "synchronization": {
        "type": "sequential",
        "distance": [
          60,
          90
        ]
      }
    }
  ],
  "services": [
    {
      "id": "s1",
      "default_duration": 30
    },      
    {
      "id": "s2",
      "default_duration": 30
    },      
    {
      "id": "s3",
      "default_duration": 30
    }
  ], 
  "caregivers": [
    {
      "id": "c1",
      "abilities": [
        "s1",
        "s2"
      ]
    },
    {
      "id": "c2",
      "abilities": [
        "s3"
      ]
    },
    {
      "id": "c3",
      "abilities": [
        "s2",
        "s3"
      ]
    }
  ],
  "central_offices": [
    {
      "id": "d",
      "location": [46.1, 13.2]
    }
  ],
  "distances": [
[0,23,22,32,50,58,39],
[23,0,44,28,47,43,35], 
[22,44,0,54,59,78,56], 
[32,28,51,0,19,28,7], 
[45,47,59,19,0,35,13], 
[57,42,77,28,35,0,27], 
[38,34,56,7,13,26,0]
  ] 
}
```

In particular, the distance matrix is assumed to be in the following order (for rows/columns): `d, p1, p2, ..., pn`. The default durations for the services has to be used if no specific value is provided for a patient requiring that service. The synchronization type can be `simultaneous` (i.e., the two services should start at the very same moment) or `sequential` (i.e., the two services should be spread apart). In the latter case the minimum and maximum amount of time the two services should distantiate is reported.

### Solution format

The [JSON type definition (RFC 8927)](https://jsontypedef.com) of the format is provided in the file `hhcrp-input-typedef.json`.

``` json
{
  "routes": [
    {
      "caregiver_id": "c1",
      "locations": [
        {
          "patient_id": "p4",
          "service_id": "s2",
          "arrival_time": 120,
          "departure_time": 150
        },
        {
          "patient_id": "p5",
          "service_id": "s1",
          "arrival_time": 275,
          "departure_time": 290
        },
        {
          "patient_id": "p6",
          "service_id": "s1",
          "arrival_time": 360,
          "departure_time": 405
        }
      ]
    },
    {
      "caregiver_id": "c2",
      "locations": [
        {
          "patient_id": "p4",
          "service_id": "s3",
          "arrival_time": 120,
          "departure_time": 150
        },
        {
          "patient_id": "p2",
          "service_id": "s3",
          "arrival_time": 178,
          "departure_time": 198
        },
        {
          "patient_id": "p6",
          "service_id": "s3",
          "arrival_time": 420,
          "departure_time": 440
        }
      ]
    },
    {
      "caregiver_id": "c3",
      "locations": [
        {
          "patient_id": "p3",
          "service_id": "s2",
          "arrival_time": 56,
          "departure_time": 101
        },
        {
          "patient_id": "p1",
          "service_id": "s2",
          "arrival_time": 240,
          "departure_time": 270
        },
        {
          "patient_id": "p5",
          "service_id": "s3",
          "arrival_time": 320,
          "departure_time": 350
        }
      ]
    }
  ],
  "global_ordering": [ 
    "p3",
    "p2",
    "p4",
    "p1",
    "p5",
    "p6"
  ]
}
```

Analogously, the solution format is described using [JSON type definition (RFC 8927)](https://jsontypedef.com) in the file `hhcrp-solution-typedef.json`.

## Instance generator

⚠️ The Instance generator and the instructions for generating new instances will be available soon.

## Instance and solution validator

The solution validator is implemented in Python. In order to use it you should install the libraries listed in the `requirements.txt` file.

The validator can be used to check instances or solutions through the `check-instance` and `check-solution` commands, for example:

``` shell
./validator.py check-instance ../instances/toy.json
```

In the event there are major errors in the instances they will be listed.

As for checking the solution, instead, also the instance which the solution refers to should be provided:

``` shell
./validator.py check-solution ../instances/toy.json ../solutions/sol_toy_optimal.json
```

A report on the solution validation will be provided, if there are validation errors they will be printed. For example, in this case the solution is valid and it is issued a report on the cost computation:

``` json
{
  "distance_traveled": 334.0,
  "tardiness": {
    "(4, 's2')": 0.0,
    "(5, 's1')": 0.0,
    "(6, 's1')": 0.0,
    "(4, 's3')": 0.0,
    "(2, 's3')": 0.0,
    "(6, 's3')": 0,
    "(3, 's2')": 0.0,
    "(1, 's2')": 0.0,
    "(5, 's3')": 0.0
  },
  "max_tardiness": 0.0,
  "total_tardiness": 0.0,
  "total_cost": 111.33333333333333
}
```


## Cite

If you use this repository in your research, please cite our paper:

### BibTeX

```bibtex
@article{ceschia2026multi,
  author = {Ceschia, Sara and Di Gaspero, Luca and Rosati, Roberto Maria and Schaerf, Andrea},
  title = {Multi-neighborhood simulated annealing for the home healthcare routing and scheduling problem},
  journal = {International Transactions in Operational Research},
  volume = {33},
  number = {1},
  pages = {38--67},
  year = {2026},
  doi = {10.1111/itor.13585},
  url = {https://doi.org/10.1111/itor.13585}
}
```

------------------------------------------------------------------------

Copyright: 2023 Intelligent Optimization Laboratory \@ Università degli Studi di Udine License: MIT
