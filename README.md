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

## Model Training & Methodology


In this project, I explored four distinct training strategies to find the best approach for detecting disaster features in aerial imagery. The development followed a logical progression where the custom models and transfer learning models built upon one another to improve accuracy:

#### 1. Custom Architectures
* **Model 1: ConvNet from Scratch** I designed a custom convolutional neural network (CNN) architecture to establish a performance baseline. This allowed me to see how a model learns raw features directly from the LADI-v2 dataset.
* **Model 2: ConvNet with Data Augmentation** **Built directly on Model 1**, I introduced a data augmentation layer (random flips, rotations, and zooms). This helped the scratch-built model generalize much better and significantly reduced the overfitting observed in the first iteration.



#### 2. Transfer Learning (ResNet50V2)
* **Model 3: ResNet50V2 Feature Extraction** I shifted to a pretrained **ResNet50V2** model to leverage advanced visual patterns. I used the base network as a fixed feature extractor, running the images through it once to generate feature arrays. This provided a massive boost in efficiency and accuracy.
* **Model 4: ResNet50V2 Fine-Tuning** **Built over the pretrained Model 3**, I unfroze the top three layers of the ResNet base. By jointly training these specific layers with the custom classifier, the model was able to "fine-tune" its weights to the unique textures of disaster imagery like debris and floodwater.



---

## Performance Summary

| Model Name | Validation Accuracy | Test Accuracy |
| :--- | :---: | :---: |
| convnet_from_scratch | 0.8627 | 0.8720 |
| convnet_from_scratch_with_augmentation | 0.8876 | 0.8680 |
| **feature_extraction_resnet50v2** | **0.8851** | **0.8900** |
| fine_tuning_resnet50v2 | 0.8791 | 0.8890 |

**Note:** The transfer learning models (3 and 4) consistently outperformed the models built from scratch, demonstrating the power of using pretrained weights for specialized disaster detection tasks.

## Performance Analysis

The final evaluation shows how the models handle real-world disaster scenarios. While the overall accuracy reached **89%**, looking deeper into the results reveals a few important insights regarding the data distribution and specific challenges.

#### Data Distribution & Challenges

* **Data Mismatch:** One of the biggest challenges was that the training and validation data were quite different from the final test data. This is common in disaster imagery where ground conditions change rapidly, but it forced the model to be more adaptable to unseen environments.
* **The "Fire" Gap:** We noticed relatively low performance in detecting fire events. This is likely due to the limited number of fire examples in the training set compared to the thousands of images available for categories like flooding or buildings.
* **Test Data Scarcity:** Because there were very few fire images in the test set, even a single misclassification significantly impacted the percentage score for that category.



<div align="left">
  <img src="model_performance_chartpng" alt="Performance Distribution Chart" width="350"/>
</div>


Despite these imbalances, the use of **Transfer Learning (ResNet50V2)** allowed the model to maintain high overall performance. By starting with a model that already understood general visual features, it remained robust even in categories where specific disaster data was scarce.

## Conclusion
We started with a basic model and slowly made it smarter by adding data tricks and eventually using a professional, pre-built model called ResNet50V2. By the end, our system became very good at looking at photos from planes and identifying things like floods, broken buildings, and blocked roads with nearly 89% accuracy.

While it still needs more practice with rare events—like fires, which were very hard to find in the data—it is a powerful tool. It shows that AI can help rescue teams find where help is needed much faster than a human could alone, even when the data isn't perfect.
 
