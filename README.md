# Classifiers-for-Low-Altitude-Disaster-Imagery
## PROJECT OVERVIEW

When a natural disaster such as a tornado, flood, fire, or severe storm occurs, emergency teams frequently deploy helicopters, drones, or low-flying aircraft to capture images of the affected regions. These aerial visuals are vital for understanding the level of destruction, highlighting damaged buildings, broken roads, flooded areas, and debris.

### The Problem
A significant challenge arises after a disaster: thousands of images are collected simultaneously. Analyzing these images manually is a time-consuming process. This delay can stall emergency response efforts and prevent critical aid from reaching the most devastated areas in a timely manner.

### The Solution
By automating the analysis of these images through computer vision, we can significantly accelerate the assessment process. This project focuses on building and evaluating classification models that determine the level of damage and identify urgent priorities. 

The goal is to provide a tool that allows emergency teams to prioritize one city or region over another based on automated data, supporting faster and more effective decision-making during crisis response.

---

## ABOUT THE DATA

### Dataset Summary: LADI-v2
The Low Altitude Disaster Imagery (LADI) v2 dataset is a specialized collection of aerial disaster images captured and labeled by the Civil Air Patrol (CAP). This dataset was created to address the shortage of well-labeled, post-disaster aerial imagery available for research.



### Why this Dataset?
The images in this dataset were specifically collected by the Civil Air Patrol (CAP) using low-altitude aircraft to capture the reality of ground conditions after a disaster. To ensure these visuals are useful for emergency response, every image is geotagged, which provides the exact location context needed for rescue and repair teams.

What makes this data particularly reliable is the expert annotation process. Trained FEMA volunteers labeled the images following standard damage assessment protocols. To maintain the highest quality for deep learning, each label was verified through a majority voting system, ensuring that the model learns from consistent and accurate information. The dataset is quite substantial, consisting of about 10,000 images totaling 55 GB, and it is pre-split into training, validation, and test sets. Ultimately, the goal is to support automatic damage detection, which significantly cuts down the time it takes for help to reach affected areas.

### Dataset Specifications
While the LADI-v2 dataset is available in various formats and sizes (ranging from a few gigabytes for resized versions to over 134 GB for the full raw archive), the version used in this project was downloaded directly via the Hugging Face datasets library.

It is sourced from MITLL/LADI-v2-dataset and is publicly accessible through the Hugging Face Repository. For this specific implementation, I have also referenced a source distribution file located at gs://codecontests-bigdata-uw/plots/source_distribution.csv to manage the data distribution and analysis.
<img src="Screenshot 1.png" alt="Dataset Sizes" width="300"/>

### Multi-Label Classification Categories
The images are labeled using multi-label classification for the following classes:

* bridges_any
* buildings_any
* buildings_affected_or_greater
* buildings_minor_or_greater
* debris_any
* flooding_any
* flooding_structures
* roads_any
* roads_damage
* trees_any
* trees_damage
* water_any