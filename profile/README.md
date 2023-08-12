# EpiCare

## Introduction

EpiCare is a collection of applications that aims to aid medical practitioners and epilepsy patients. The application introduces a platform for patients and doctors that offers a variety of services, like evaluating EEG signals through ML models, real-time tracking of patient health, and tracking medical history, and many more.

## Application Structure

![Project Structure](https://github.com/Epilepsy-Detection/.github/assets/23122237/bf0b4b70-b8fa-40df-ac85-18ed2725cc31)


The app consists of smaller applications (micro-services) that aim to separate the logic of the application and ease the process of scaling specific features to adjust to the load on the system.

### [Frontend Application](https://github.com/Epilepsy-Detection/frontend)

Epicare’s front end is the portion of the application with which the user (either a doctor or a patient) interacts. It solicits the necessary user input and displays the desired outcomes. Doctors use our website to connect with their patients and view their EEG data in real-time. Doctors also use it to monitor their patients and view a patient’s medical history. On the other side, a patient can utilize the system to view his or her medical history and modify his or her personal information.

Stack: Javascript, ReactJS, Redux

### [API Server](https://github.com/Epilepsy-Detection/ep-det-api)

![api structure](https://github.com/Epilepsy-Detection/.github/assets/23122237/e919d215-37fb-4ab0-bce8-08dfcd5324b2)


The API Server handles the REST HTTP Requests to our Backend server. The Frontend application invokes API requests to the server to obtain the required functionalities.

- Authentication (Login, Register)
- Role Based Authorization (Doctor, Patient)
- Edit your Profile and upload a profile photo
- Upload EEG Signals and generate reports for patient
- Manage patient report history


The API server sends the EEG signal file uploaded by the doctor to the Machine Learning server to obtain the results of evaluating the signal, then it uploads the signal to the AWS S3 bucket to be obtained later for previous processing by the doctor.


Stack: Javascript, NodeJS, ExpressJS, MongoDB


### [Machine Learning Server](https://github.com/Epilepsy-Detection/ml-model-webserver)

![ml server api](https://github.com/Epilepsy-Detection/.github/assets/23122237/c2a1e4c6-bc00-425e-8632-ea276a56a1d8)

A Python web application that offers an endpoint to evaluate an EEG signal and return the results based on the evaluation of the Machine Learning server.

The application is containerized using docker to allow ease of management of dependencies and offload the load of downloading TensorFlow and its dependencies.

Stack: Python, Flask, Docker, Tensorflow


### [Real-time Server](https://github.com/Epilepsy-Detection/ep-det-realtime)

![realtime](https://github.com/Epilepsy-Detection/.github/assets/23122237/7f366590-13c5-462a-af9f-7e0daacb80b0)


When the patient is wearing a device that transmits EEG data continuously. A system is needed to analyze the data the patient is sending. After analyzing the data the system needs to notify an emergency contact if the patient is suffering from a seizure. Also, the doctor needs to read the data on his website dashboard and see a live graph of the actual data.


- Handle continuous real-time EEG data sent from the patient
- Event-based triggered service that notifies emergency contacts using SMS or phone call
- Parse data and send it to the front-end application to be graphed for the doctor
- Evaluate data efficiently in real-time using machine learning models

Stack: NodeJS, SocketIO, Redis, RabbitMQ


### [Notifier Server](https://github.com/Epilepsy-Detection/ep-det-notifier)

![notifier rabbit](https://github.com/Epilepsy-Detection/.github/assets/23122237/1669c294-05fa-464e-9671-deff69e9a085)

The Emergency Contact Notifier Server is a server-side JavaScript application designed to provide a crucial notification and messaging service to the emergency contacts of a patient in need. Its primary function is to leverage the capabilities of the WhatsApp Cloud API to send real-time updates to the designated emergency contacts, ensuring they stay informed about the current situation of the patient.

To establish communication between the Emergency Contact Notifier Server and the real-time  Server, a RabbitMQ message queue is employed. The notifier server listens to this message queue, waiting for incoming messages from the real-time Server that indicate a patient is currently experiencing a seizure. 

This integration with RabbitMQ ensures reliable and asynchronous communication between the servers. The use of RabbitMQ as the messaging platform allows for efficient and scalable message exchange, enabling seamless coordination between the real-time Server and the Emergency Contact Notifier Server. 

Stack: NodeJS, Redis, RabbitMQ
