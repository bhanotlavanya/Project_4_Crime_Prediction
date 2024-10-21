# Crime Data Analysis Project

## Overview

This project focuses on analyzing crime data from 2021 to 2024 using data obtained from the Major Crime Indicators (MCI) dataset in Toronto. The data is retrieved from an ArcGIS REST API, processed, and prepared for further analysis to understand crime patterns in the city. The analysis aims to examine different types of crimes, including Assault, Auto Theft, Break and Enter, Robbery, and Theft Over, along with various other attributes like occurrence date, location, and premises type. The ultimate goal is to help law enforcement agencies allocate resources more effectively and develop proactive strategies to prevent crime.

## Data Retrieval

Crime data is retrieved from the ArcGIS REST API, which provides a GeoJSON-formatted response containing information about crime incidents. The data is fetched in batches due to API limitations on the number of records that can be retrieved in a single request. The process involves:

1. **API Calls**: Iteratively making API calls with a `resultOffset` parameter to retrieve up to 2000 records at a time.
2. **Filtering**: Filtering the data for incidents that occurred between 2021 and 2024.

## Data Processing

The retrieved GeoJSON data is processed using `pandas` to create a structured DataFrame for further analysis:

1. **Conversion to DataFrame**: The JSON array is converted into a `pandas` DataFrame for easy data manipulation.
2. **Selecting Relevant Columns**: Only necessary columns such as `EVENT_UNIQUE_ID`, `NEIGHBOURHOOD_158`, `LAT_WGS84`, `LONG_WGS84`, `PREMISES_TYPE`, `OCC_DATE`, `OCC_YEAR`, `OCC_MONTH`, `OCC_DAY`, `OCC_HOUR`, and `MCI_CATEGORY` are retained for analysis.
3. **Encoding Crime Categories**: The `MCI_CATEGORY` column is one-hot encoded to create binary columns for each crime category (Assault, Auto Theft, Break and Enter, Robbery, Theft Over).

## Data Aggregation

The processed data is further grouped and aggregated for better analysis:

1. **Grouping by Event ID**: The data is grouped by `EVENT_UNIQUE_ID` to consolidate information related to the same crime incident. The grouping is done to get:
   - The first occurrence of attributes such as `NEIGHBOURHOOD_158`, `LAT_WGS84`, `LONG_WGS84`, `PREMISES_TYPE`, `OCC_DATE`, `OCC_YEAR`, `OCC_MONTH`, `OCC_DAY`, and `OCC_HOUR`.
   - The maximum value of the one-hot encoded crime categories for each event to identify which crime types are associated with the incident.

2. **Merging Groups**: The results from the grouped DataFrames are concatenated to form a final DataFrame, which is then reset to have a numeric index.

## Example Data

The final processed DataFrame includes the following columns:
- `EVENT_UNIQUE_ID`: Unique identifier for each crime incident.
- `NEIGHBOURHOOD_158`: Neighbourhood code where the crime occurred.
- `LAT_WGS84`, `LONG_WGS84`: Geographical coordinates of the incident.
- `PREMISES_TYPE`: Type of premises where the crime occurred.
- `OCC_DATE`, `OCC_YEAR`, `OCC_MONTH`, `OCC_DAY`, `OCC_HOUR`: Date and time details of the occurrence.
- `Assault`, `Auto Theft`, `Break and Enter`, `Robbery`, `Theft Over`: One-hot encoded crime categories indicating the type of crime.

## Prerequisites

- **Python 3.x**: The code requires Python 3.x to run.
- **Required Libraries**:
  - `pandas`
  - `requests`

