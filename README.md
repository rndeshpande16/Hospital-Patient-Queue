# MediRoute v2.0: Intelligent Hospital Management System

Solves following problems:

- Assigns priority score based on severity of patient from his/her past medical details by reading PDF (1 --> High severity --- 5 --> Low severity)
- Automatically decides who doctor should attend to first
- Ambulance Shortest Path recommendation system works entirely offline and can be used in places with low reach of the internet

## Ward Queue (Max-Heap)

python -m pip install Flask && python app.py → open http://127.0.0.1:5000

- heap.py : indexed max-heap (sift-up/down, extract-max, remove, update) + hash map
- app.py : patient records (dict) + Flask API
- templates/ + static/ : frontend
  Ranking: higher urgency first, ties go to the earlier arrival.

## Smart Priority Assignment System Algorithm (Linear Regression + OCR)

```mermaid
flowchart TD
    StartNode([Download a dataset containing parameters such as past visits, comorbidities, blood pressure, age, bmi, spo2, etc.]) ==> B[Load & process data, extract features, and engineer them according to the change in urgency levels]
    B ==> C[Train the model using Softmax Regression Algorithm from Tensorflow]
    C ==> D[Test the model using train_test_split - 90% training and 10% testing - and calculate accuracy scores]
    D ==> E[Using pymupdf library, read standard format uploaded pdf containing recent patient medical details
    Convert them to text and process data into given features and use the model to predict the priority score for the given patient
    Use softmax regression for outputs in range 1-5 by shifting down the range by 1 -> 0-4]
    E ==> EndNode([Output the priority in range 1-5 to the heap used in the flask server backend])
```

## Model Accuracy Statistics:

- Macro Average Accuracy: 84%
- Precision: Average Precision of 94% for critical patients
- Recall:
  - 95% for priority 1 patients (most critical)
  - 95% for priority 2 patients (critical)
  - `This very fact makes the model quite accurate and suitable for real-world deployment.`

## Ambulance Shortest Path Recommending System (BFS + Dijkstra)

```mermaid
flowchart TD
    StartNode([Set a matrix of roads around the city]) ==> B[(Set the coordinates of the places around the city)]
    B ==> C[Use BFS + Dijkstra to find shortest path from source to destination avoiding obstacles - 0 in matrix]
    C ==> EndNode([Display the length of the shortest path and the path on a map / graph / plot])
```

### Tests Run On Source Hospital - Dadar & 5 locations:

#### Source:

- Hospital at (0, 1)

#### Destinations:

- A at (0, 4)
- B at (9, 8)
- C at (0, 8)
- D at (8, 5)
- E at (3, 6)

<h3 align='right'>Authored by: </div>

<div align='right'>Ruturaaj Deshpande (SY B.Tech IT, VJTI 29')</div>
<div align='right'>Eashan Thakur (SY B.Tech IT, VJTI 29')</div>
