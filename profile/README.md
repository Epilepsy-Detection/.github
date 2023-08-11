# EpiCare

## Introduction

EpiCare is a collection of applications that aims to aide medical practitioners and epilepsy patients. The application introduces a platform for patients and doctors that offers a variety of services, like evaluating EEG signals through ML models, and real-time tracking of patient health, and tracking medical history, and many more.

## Application Structure

## PIC GOES HERE

The app consists of smaller applications (micro-services) that aims to seperate the logic of the application and ease the proccess of scaling specific features to adjust to the load on the system.

### Frontend Application

Epicare’s frontend is the portion of the application with which the user (either a doctor or a patient) interacts. It solicits the necessary user input and displays the desired outcomes. Doctors use our website to connect with their patients and view their EEG data in real time. Doctors also use it to monitor their patients and view a patient’s medical history. On the other side, a patient can utilize the system to view his or her medical history and modify his or her personal information.

Stack: Javascript, ReactJS, Redux

### API Server

## PIC GOES HERE

The API Server handles the REST HTTP Requests to our Backend server. The Frontend application invokes API requests to the server to obtain the required functionalites.

- Authentication (Login, Register)
- Role Based authorization (Doctor, Patient)
- Edit Profile and uploading profile photo
- Upload EEG Signals and generate reports for patient
- Manage patient report history


The API server sends EEG signal file uploaded by the doctor to the Machine Learning server to obtain the results of the evaluating the signal, then it uploads the signal to AWS S3 bucket to be obtained later for previous processing by the doctor.


Stack: Javascript, NodeJS, ExpressJS, MongoDB


### Machine Learning Server

## PIC GOES HERE

A Python web application that offers an endpoint to evaluate an EEG signal and return the results based on the evaluation of the Machine Learning server.

The application is containerized using docker to allow ease managment of dependencies and offload the load of downloading TensorFlow and its dependencies.

Stack: Python, Flask, Docker, Tensorflow


### Real-time Server

When the patient is wearing a device that transmits EEG data continuously. A system is needed to analyze the data the patient is sending. After analyzing the data the system needs to notify an emergency contact if the patient is suffering from a seizure. Also, the doctor needs to read the data on his website dashboard and see a live graph of the actual data.

## PIC GOES HERE

- Handle continuous real-time EEG data sent from the patient
- Event-based triggered service that notifies the emergency contacts using SMS or phone call
- Parse data and send it to the front-end application to be graphed for the doctor
- Evaluate data efficiently in real-time using machine learning models

Stack: NodeJS, SocketIO, Redis, RabbitMQ


### Notifier Server

The Emergency Contact Notifier Server is a server-side JavaScript application designed to provide a crucial notification and messaging service to the emergency contacts of a patient in need. Its primary function is to leverage the capabilities of the WhatsApp Cloud API to send real-time updates to the designated emergency contacts, ensuring they stay informed about the current situation of the patient.

To establish communication between the Emergency Contact Notifier Server and the real-time  Server, a RabbitMQ message queue is employed. The notifier server listens to this message queue, waiting for incoming messages from the real-time Server that indicate a patient is currently experiencing a seizure. 

This integration with RabbitMQ ensures reliable and asynchronous communication between the servers. The use of RabbitMQ as the messaging platform allows for efficient and scalable message exchange, enabling seamless coordination between the real-time Server and the Emergency Contact Notifier Server. 

Stack: NodeJS, Redis, RabbitMQ
