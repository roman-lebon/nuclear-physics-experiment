# Nuclear physics experiment

## Highlights

- Implements object-oriented design using separate `Source` and `Detector` classes
- Uses interface (`.h`) and implementation (`.cpp`) files for class definitions
- Reads experimental configuration from `config.txt`
- Stores objects using `std::vector`
- Simulates radiation measurements using randomised detector counts
- Validates configuration file input and handles invalid data safely
- Graceful error handling for incorrect configuration formatting

## Overview

This program simulates a simple nuclear physics experiment consisting of radioactive sources and radiation detectors.

The program reads configuration data from a structured text file (`config.txt`) and creates the corresponding objects in memory.

Each **source** object contains:
- A radioactive isotope type
- A date of acquisition
- The source activity (at the time of acquisition), in Bq/kg
- A unique identifier 

Each **detector** object contains:
- A detector type
- A detector status (on/off)
- The number of counts detected

## Usage

When executed, the program reads the experimental configuration from `config.txt`
and performs the following steps:

- Parses the configuration file line by line
- Creates `Source` and `Detector` objects from the configuration data
- Stores the objects in `std::vector` containers
- Prints the detector information
- Iterates through all sources and prints their characteristics
- Passes each source to the detector
- Simulates a measurement and outputs the detected counts
- The process is repeated for each detector defined in the configuration file

The simulation therefore models the interaction between radioactive sources and radiation detectors using object-oriented programming principles.

Example configuration file:

```text
SOURCE Cs-136 2026-03-14 50 1
SOURCE Na-22 2026-03-14 40 2
SOURCE Co-92 2026-03-14 30 3
DETECTOR Geiger 1
```

Example output:

```text
===============================================================================
Detector Information:
Type: Geiger
Status: ON
-------------------------------------------------------------------------------
Source information:
Type: Cs-136
Date: 2026-03-14
Activity: 50 Bq/kg
Source ID: 1
Counts detected: 938
-------------------------------------------------------------------------------
-------------------------------------------------------------------------------
Source information:
Type: Na-22
Date: 2026-03-14
Activity: 40 Bq/kg
Source ID: 2
Counts detected: 946
-------------------------------------------------------------------------------
-------------------------------------------------------------------------------
Source information:
Type: Co-92
Date: 2026-03-14
Activity: 30 Bq/kg
Source ID: 3
Counts detected: 872
-------------------------------------------------------------------------------
===============================================================================
```

## Implementation Details

### Class Structure

The program models the experiment using two classes:

- `Source`
- `Detector`

Class interfaces are defined in header files:

```text
source.h
detector.h
```
Implementations are contained in:

```text
source.cpp
detector.cpp
```

The main simulation logic is implemented in:

```text
assignment-3.cpp
```

### Source Class

The `Source` class represents a radioactive source used in the experiment.

Data members:

```cpp
std::string type;
std::string date;
double activity;
int id;
```

The class provides constructors, getters, setters, and a `print_source()` function for displaying source information.

Validation ensures that source types are recognised, activity values are non-negative, identifiers are valid, and acquisition dates are not empty.

### Detector Class

The `Detector` class represents a radiation detector.

Data members:

```cpp
std::string type;
bool status;
int counts;
```

The class provides constructors, getters, setters, and a `print_detector()` function.  
The `detect(Source)` function simulates a measurement by generating a random number of counts from 0-999 when the detector is switched ON. If the detector is OFF, zero counts are returned.

```cpp
counts = rand() % 1000;
```

### Data Storage

Source and detector objects are stored using standard containers:

```cpp
std::vector<Source> sources;
std::vector<Detector> detectors;
```

Each line of the configuration file is parsed using `std::stringstream`:

```cpp
std::stringstream ss(line);
ss >> tag;
```

The tag determines whether the line describes a `SOURCE` or a `DETECTOR`.

## Design Decisions

Several design choices were made to make the simulation flexible and easy to modify.

### Representation of the Experiment

The program models a simplified nuclear physics experiment by representing the physical components as objects. Radioactive sources are represented by the `Source` class, while measurement devices are represented by the `Detector` class. This mirrors the structure of a real experimental setup where detectors measure radiation emitted by sources.

### Extending Source Types

The assignment required support for several radioactive isotopes (Na-22, Cs-136, Co-92). The implementation extends this list using a vector of valid sources, including additional commonly used isotopes such as Cs-137, Co-60, Am-241, and Sr-90. 

Examples of commonly used radioisotopes were taken from:
https://world-nuclear.org/information-library/non-power-nuclear-applications/radioisotopes-research/radioisotopes-in-industry

This approach allows new isotopes to be added easily by modifying the validation list.

### Extending Detector Types

Similarly, the detector list was expanded beyond the minimal assignment requirement to include additional radiation detectors such as silicon detectors, proportional counters, and ionisation chambers.

This allows the configuration file to describe a wider range of experimental setups.

## Configuration File Format

The experimental setup is defined in the file `config.txt`.  
Users can modify this file to simulate different experimental configurations without changing the program source code.

Each line in the configuration file defines either a radioactive **source** or a **detector**.

### Source Entries

Source objects must follow the format:

```
SOURCE <type> <date> <activity> <id>
```

Example:

```
SOURCE Cs-136 2026-03-14 50 1
```

Where:

| Parameter | Description |
|---|---|
| `type` | Radioactive isotope (must be in the recognised source list) |
| `date` | Acquisition date in the format `YYYY-MM-DD` |
| `activity` | Source activity in Bq/kg (must be non-negative) |
| `id` | Unique non-negative integer identifier |

### Detector Entries

Detector objects must follow the format:

```
DETECTOR <type> <status>
```

Example:

```
DETECTOR Geiger 1
```

Where:

| Parameter | Description |
|---|---|
| `type` | Detector type (must match a recognised detector) |
| `status` | Detector status: `1` for ON or `0` for OFF |

### Formatting Rules

To ensure the configuration file is parsed correctly:

- Each object must appear on a **separate line**
- Each line must begin with either `SOURCE` or `DETECTOR`
- Parameters must be separated by **spaces**
- Extra parameters or incorrect formatting will cause the program to terminate with an error message
- At least **one source and one detector** must be defined in the file

## Error Handling

The program terminates safely in the following cases:

- Configuration file cannot be opened
- Incorrect configuration formatting
- Invalid source type
- Invalid detector type
- Invalid date format
- Negative activity values
- Invalid detector status
- Missing source or detector entries
- Duplicate source identifiers

Graceful termination is performed using:

```cpp
return 1;
```

Error messages are printed to explain the cause of failure.

## Compilation

Tested using g++11 as required.

Compile:

```bash
g++ -std=c++11 -o assignment3 assignment-3.cpp detector.cpp source.cpp
```

Run:

```bash
./assignment3
```

## Development Process

The program was developed incrementally, with features committed separately to GitHub. The commit history reflects the following progression:

1. Created the initial `Source` class structure.
2. Implemented constructors, getters, setters, and print functionality for the source.
3. Implemented the `Detector` class structure.
4. Added detector status handling and output formatting.
5. Implemented the `detect()` function to simulate measurements.
6. Added validation for valid detector and source types.
7. Implemented reading of experimental configuration from `config.txt`.
8. Added validation for configuration file formatting.
9. Implemented date validation and input checking.
10. Added loops to iterate over all sources and detectors.
11. Improved output formatting and error messages.
12. Tested program behaviour with valid and invalid configuration files.
13. Added validation to ensure that source identifiers in the configuration file are unique.

Each stage corresponds to one or more commits in the Git history.

## Use of Generative AI

This assignment was completed without the use of generative AI tools.  
All program code and documentation, including this README, were composed independently.

## Author

Roman le Bon  
University of Manchester
