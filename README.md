# DALVacationHome-Serverless-Multicloud-Platform

## Overview

**DALVacationHome-Serverless-Multicloud-Platform** is a **serverless** application designed for managing vacation home room bookings and property management. Utilizing a **multi-cloud architecture** with **AWS** and **GCP**, the platform delivers a seamless experience across various user types.

- **Guests:**
  - Browse room availability and tariffs
  - Review room feedback and ratings
  - Use a virtual assistant for navigation and basic queries

![Guest User - View Rooms](https://github.com/user-attachments/assets/8c7c8655-6ae6-43c1-bad8-179726059f5c)

![Room Available](https://github.com/user-attachments/assets/df896560-578d-454b-b2e6-a3b21f8494b3)

- **Registered Customers:**
  - Access all guest features
  - Make bookings for rooms or recreation spaces for specified dates
  - Receive notifications regarding registration, sign-ins, and booking status (success or failure)
  - Interact with the virtual assistant to retrieve booking details or submit tickets for booking-related issues

- **Property Agents:**
  - Manage room listings (add, edit, or remove rooms)
  - Set and update room prices and features
  - Receive email notifications about customer concerns
  - Access detailed analytics and customer feedback

![Property Agent - ADD ROOM](https://github.com/user-attachments/assets/98e291fc-5e11-4710-abe9-0cd52985dd33)

![Property Agent - EDIT ROOM](https://github.com/user-attachments/assets/257f27e0-402c-4834-bef4-15072c920251)

![Property Agent - DELETE ROOM](https://github.com/user-attachments/assets/d324dc29-5f55-4892-9fef-27a0973daee7)


## Tech Stack & Services

- **Front-End**: `React.js`, deployed using `GCP Cloud Run`
- **Back-End Processing**: `AWS Lambda` and `GCP Cloud Functions` (written in `Python` and `Node.js`), `GCP Pub/Sub`
- **Database**: `DynamoDB`
- **Authentication**: `AWS Cognito` (Multi-factor Authentication)
- **Virtual Assistant**: `Google Dialogflow`
- **Notifications**: `AWS SNS`, `AWS SQS`
- **Data Analysis & Visualization**: `Google Natural Language API`, `Looker Studio`
- **Infrastructure as Code (IaC)**: `AWS CloudFormation`

## Architecture

![Architecture](https://github.com/user-attachments/assets/f8d88409-9032-469f-a8c1-3145ca9abd67)

## Modules

### 1. User Management & Authentication Module
  
This module manages user registration and login with three layers of authentication.

**Registration:**
- Users can register by providing their details on the 'Register' Page

![Register 1](https://github.com/user-attachments/assets/69fd46e0-0843-4727-8302-c05204ae214f)

**Login Process:**
- **Step 1**: Users log in with their email and password.

![Login 1](https://github.com/user-attachments/assets/f7921bb1-3d6a-4ef2-b02b-092c511b3ef8)
- **Step 2**: Users answer a security question set by them.

![Login 2 - Security Quesion](https://github.com/user-attachments/assets/2c8692fa-adb9-4b3b-883e-092266106dc8)

- **Step 3**: Users solve a Caesar cipher challenge.

![Login 3 - CAESAR CIPHER](https://github.com/user-attachments/assets/1399ca16-c703-4852-9df6-39bf56c114c2)


**Services:**
- **User ID/Password (1st factor authentication)** – `AWS Cognito`
- **Question/Answer (2nd factor authentication)** – `DynamoDB` + `AWS Lambda`
- **Caesar cipher (3rd factor authentication)** – `AWS Lambda` + `DynamoDB`

### 2. Virtual Assistant Module

The application provides a virtual assistant designed to enhance user experience by handling various queries and assisting with site navigation.

![Virtual Assistant](https://github.com/user-attachments/assets/d1c433ca-5123-4e5c-9037-78a15080f72f)

**Functionality:**
- **Site Navigation**: Guides users through the website and answers questions such as “how to register?”

![Basic Navigation](https://github.com/user-attachments/assets/e2093973-e0af-465c-80f6-726465dd3fc0)

- **Booking Information**: Retrieves booking details for registered customers when provided with a booking reference.

![Booking Details](https://github.com/user-attachments/assets/d329948a-6fbf-4cc0-ac20-f049fae362aa)

- **Customer Concerns**: Accepts and processes customer concerns about bookings, creating tickets that are forwarded to property agents for resolution.

![Booking Concern1](https://github.com/user-attachments/assets/ddfb5c7d-3e82-47f0-b5c0-6847bda01edb)

![Booking Concern2](https://github.com/user-attachments/assets/b8f34856-c9f6-4486-8165-cd989812aab1)


**Services:**
- **Chatbot** - `Google Dialogflow`
- **Site Navigation** - `Google Dialogflow`
- **Booking Information** - `DynamoDB` + `AWS Lambda`
- **Customer Concerns** - `DynamoDB` + `AWS Lambda`

### 3. Message Passing Module

This module facilitates communication between customers and property agents. When a customer raises a concern via the virtual assistant, the ticket is assigned to a random property agent in the system for answering.

**Functionality:**
- **Agent Notification**: The randomly assigned property agent receives an email with the ticket details and instructions to contact the customer and resolve the issue.

![Agent Email](https://github.com/user-attachments/assets/6b24cce7-e47a-4d90-be14-ee31623e6da5)

- **Customer Notification**: The customer is notified via email that an agent has been assigned to their ticket, including the agent's contact details.

![Customer Email](https://github.com/user-attachments/assets/0d43f11d-5aa9-467a-a27d-8c5bc8251020)

**Services:**
- **Agent Assignment** - `GCP Pub/Sub` + `AWS Lambda` + `GCP Cloud Functions`
- **Email Notifications** - `AWS Lambda`

### 4. Notifications Module

Handles email notifications related to user authentication and booking processes.

**Functionality:**
- **Successful Registration/Login Notifications**: Users receive email confirmations upon successful registration and login.

![Successful Login](https://github.com/user-attachments/assets/d1684d04-ff30-461d-bf86-b0eaabe4e642)

- **Booking Request Submission**: When a booking request is made, it is placed into a queue.

![Booking Request Sent](https://github.com/user-attachments/assets/7b8f6e4c-166c-4f0d-8540-e0a047c60ce7)

- **Processing and Notifications**: The booking request is processed from the queue, and users receive email notifications about booking approval or failure.

![Booking Confirmation Email](https://github.com/user-attachments/assets/f69553b1-de9a-4dc9-b693-87fceb8cead9)

**Services:**
- **Registration/Login Notifications**: `AWS Simple Notification Service (SNS)`
- **Processing of Booking Requests**: `AWS Simple Queue Service (SQS)` + `AWS Lambda`
- **Booking Success/Failure Notifications**: `AWS Simple Notification Service (SNS)`

### 5. Data Analysis & Visualization Module

Provides insights and analytics for users and property agents, helping them make informed decisions.

**Functionality:**

- **Customer Feedback**:
  - Registered customers can submit reviews and ratings for rooms.

![Room Review](https://github.com/user-attachments/assets/f3b213a3-b74e-4d0d-817e-37d78327a2f6)

  - Feedback is analyzed to determine sentiment and displayed with a polarity score.

![Feedback Polarity Reviews](https://github.com/user-attachments/assets/738bd467-f0f5-4a1f-93ec-ad37a90af36f)

- **Property Agent Analytics**:
  - Property agents can view detailed feedback and usage statistics via an embedded Looker Studio dashboard.

![Analytics - Looker Studio](https://github.com/user-attachments/assets/95397587-5fbf-424f-bb74-49073c4cf32e)

![Analytics - Looker Studio (Usage Stats)](https://github.com/user-attachments/assets/74e0bcab-4404-4c6c-924d-e0b17120fe50)

**Services:**
- **Feedback Sentiment Analysis:** `Google Natural Language API`
- **Data Visualization & Statistics:** `Looker Studio`

## Deployment

Access the live application [here](https://ni-fe5byrxnta-uc.a.run.app/).

The application is deployed using `Google Cloud Run`, which allows us to run containerized applications in a fully managed environment.