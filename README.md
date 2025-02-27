# Drift Detection Algorithms in Federated Learning: FedDrift and FedDrift Eager

![image](https://github.com/user-attachments/assets/7d0916c9-b7de-4c07-acd0-ef999d95d17d)

## Introduction
The main goal of Federated Learning is training local models and taking the aggregate of weights of those models to update the central model or the global model at the server at the end of each communication round.

### Facts about Federated Learning:
- A central server coordinates the training process but only receives model updates (gradients or weights) rather than raw data. Lower communication bandwidth is required since only model updates are communicated, not raw data.
- Privacy is a primary concern. Raw data never leaves the local devices, which helps protect user privacy.
- Ideal for applications where data privacy is crucial, such as medical data analysis, personalized recommendations, and mobile applications.

### An Example of Federated Learning Implementation:
- **Federated Learning**: Mobile devices train local models on user data and send updates to a central server for aggregation, commonly used by companies like Google for improving personalized services while safeguarding user privacy.
- **Distributed Training Frameworks**: Utilizing tools like Apache Spark or TensorFlow Distributed for scalable model training.

This renders traditional methods useless as they require huge amounts of data to be windowed and checked. This is not possible in federated learning as each client device has its own smaller local dataset. This information is sufficient to train the model locally and send back its weights to the server.

Therefore, we examine concept drift in Federated Learning, where data is heterogeneous over time and also over the clients. The following algorithms concentrate on multiple and simultaneous model training in clients while comparing adaptive FedAverage and the losses of that particular model from the previous round. The **FedDrift** algorithm is designed to address this issue by dynamically adapting to the changes in data distribution across different clients over time.

## The Two Algorithms:
- **FedDrift**
- **FedDrift Eager**

## FedDrift Eager: Proactive Adaptation in Federated Learning
The FedDrift framework houses the **FedDrift Eager** algorithm, specifically designed to tackle concept drift proactively within federated learning environments. This algorithm leverages a multi-model approach to accommodate the emergence of various concepts over time.

### The Objective: Mitigating Concept Drift
The **FedDrift Eager** algorithm strives to minimize the loss function across clients and time. It achieves this by training models adept at handling changes in data distributions caused by concept drift. This necessitates a system where models specialize in handling specific data distributions and dynamically adjust to new concepts as they arise. The ultimate goal is to guarantee robust model performance even in the face of fluctuating and evolving data distributions.

### Single vs. Multiple Model Approaches: A Comparison
- **Single Model Approach**: Utilizes a single global model for inference on all clients, minimizing the average loss across all clients and time periods. However, it struggles in scenarios with significant concept drift.
- **Multiple Model Approach**: Trains numerous global models for distinct concepts. It dynamically groups clients and assigns each cluster the most suitable model for inference, minimizing loss and providing a more robust solution for handling concept drift.

### FedDrift Eager: A Multi-Step Process
1. **Client Clustering**: Clients are grouped based on their current data distribution.
2. **Model Assignment**: Each client is assigned a model based on the one that yields the lowest loss on its local data.
3. **Local Updates**: Clients perform local updates on their designated models using their local data before sending these updates to the server.
4. **Server Aggregation**: The server aggregates these updates to refine the global models.
5. **Drift Detection**: The algorithm incorporates a mechanism for detecting concept drift and creating new clusters as needed.

### Advantages of FedDrift Eager
- **Proactive Adaptation**: Adapts to concept drift by creating new models and clusters as needed.
- **Improved Performance**: Ensures effectiveness even when data distributions change.
- **Scalability**: Handles a large number of clients with heterogeneous data distributions efficiently.

## FedDrift: Combating Concept Drift in Federated Learning
The **FedDrift** algorithm employs a proactive, multi-model approach to effectively adapt to new concepts that surface over time. The core of **FedDrift** lies in dynamic client clustering, model assignment, and continuous vigilance against concept drift. It fosters the creation of new clusters and models on-the-fly, ensuring sustained model performance.

### Single vs. Multiple Model Approaches: A Tale of Two Strategies
- **Traditional Single Model Approach**: Uses one global model for all clients, minimizing average loss but failing in heterogeneous and dynamic data distributions.
- **FedDrift Multiple Model Approach**: Trains multiple global models, dynamically clusters clients, and assigns the most suitable model for inference, effectively handling concept drift.

### FedDrift in Action: A Step-by-Step Breakdown
1. **Creation of Global Models**: Multiple global models are created, each representing a distinct concept.
2. **Loss Computation**: Clients compute the loss for every model on their local data.
3. **Model Assignment**: Clients assign themselves to the model with the lowest loss.
4. **Local Training**: Clients perform local training on their assigned model using their local data.
5. **Model Update Transmission**: Updated model parameters are transmitted to the central server.
6. **Aggregation and Drift Detection**: The server aggregates model updates and detects significant deviations in data distribution.
7. **Client Reassignment and New Models**: If drift is detected, new clusters are formed, clients are reassigned, and new models are initialized.
8. **Updated Models Distribution**: Updated global models are disseminated to clients for the subsequent training round.

### Advantages of FedDrift
- **Proactive Adaptation**: Dynamically creates new models and clusters to adapt to concept drift.
- **Enhanced Performance**: Uses multiple models specialized for different concepts to improve overall performance.
- **Scalability**: Handles a large number of clients with diverse and evolving data distributions efficiently.

## Conclusion
The **FedDrift** algorithm stands out as a powerful and scalable solution for tackling concept drift in federated learning environments. By dynamically clustering clients and training multiple models, it ensures that the trained models remain effective despite fluctuating data distributions. This approach proves to be pivotal for real-world federated learning applications where data is **non-IID** and susceptible to frequent concept drifts, ensuring that models can adapt and perform well in dynamic environments.

## Summary
- **FedDrift** employs a bottom-up approach that isolates clients detecting drift and merges clients iteratively corresponding to the same concept.
- **FedDrift Eager** is a special case where only one new concept emerges at a time.
- Applying a drift detection test globally at a server aggregating the errors leads to poor performance.

