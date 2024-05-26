### Requirements

The requirements for the Bachelor Home Management System (BHMS) are categorized using the MoSCoW prioritization method:

#### Must Have
1. **Expense Management:**
   - Track food market expenses and other communal expenses.
   - Divide expenses among all residents.
   - Generate detailed expense reports.

2. **Meal Management:**
   - Track daily meal consumption.
   - Calculate meal prices based on market expenses.
   - Provide a monthly meal price breakdown.

3. **Market Duty Roster:**
   - Assign market duty to residents.
   - Create a roster to define who is responsible for purchasing groceries.

4. **User Interaction:**
   - Easy-to-use interface tailored for users in Bangladesh.
   - Support for both English and Bengali languages.

5. **Reports Generation:**
   - Generate monthly and weekly reports of expenses and meal consumption.
   - Export reports in PDF format.

6. **Notifications:**
   - Notify users about their duties, pending payments, and other important updates.

#### Should Have
1. **User Roles:**
   - Admin: Manage users, assign roles, oversee expenses.
   - User: Track personal expenses, view reports, manage meals.

2. **In-App Notifications:**
   - Real-time notifications within the app.

#### Could Have
1. **Additional Language Support:**
   - Potential support for more languages based on user demand.

2. **Mobile Responsiveness:**
   - Ensure the web app is mobile-friendly for better accessibility.

#### Won’t Have
1. **Advanced Analytics:**
   - Complex data analysis and forecasting features will not be included in the initial version.





/////////////////////////////////////////////////////////////////////////////////////////////////////////////
### Requirements V2



#### Must Have
1. **Expense Management:**
   - RESTful APIs to track food market expenses and other communal expenses.
   - Divide expenses among all residents.
   - Generate detailed expense reports via API.

2. **Meal Management:**
   - RESTful APIs to track daily meal consumption.
   - Calculate meal prices based on market expenses.
   - Provide a monthly meal price breakdown via API.

3. **Market Duty Roster:**
   - RESTful APIs to assign market duty to residents.
   - Create a roster to define who is responsible for purchasing groceries.

4. **User Management:**
   - JWT-based authentication and authorization.
   - APIs to manage user roles (Admin and User).
   - Multilingual support for English and Bengali via API.

5. **Reports Generation:**
   - Generate monthly and weekly reports of expenses and meal consumption.
   - Export reports in PDF format via API.

6. **Notifications:**
   - APIs to notify users about their duties, pending payments, and other important updates via email/SMS.

#### Should Have
1. **In-App Notifications:**
   - Real-time notifications within the app using WebSockets.

2. **Database Management:**
   - Efficient MongoDB schema design to handle large datasets.
   - Indexing for faster query performance.

#### Could Have
1. **Additional Language Support:**
   - APIs to support more languages based on user demand.

2. **Mobile Responsiveness:**
   - Ensure APIs are optimized for mobile app consumption.

#### Won’t Have
1. **Advanced Analytics:**
   - Complex data analysis and forecasting features will not be included in the initial version.

### Method

The following section outlines the technical method of addressing the requirements for the Bachelor Home Management System (BHMS).

#### Architecture Overview

The BHMS backend will follow a modular and scalable architecture, leveraging the MERN stack components (MongoDB, Express.js, React.js, Node.js) with a focus on the backend. 

```plantuml
@startuml
actor User
actor Admin

User --> WebApp: Uses
Admin --> WebApp: Manages

WebApp --> API: API Requests
API --> AuthService: Authenticates
API --> ExpenseService: Manages Expenses
API --> MealService: Manages Meals
API --> DutyService: Manages Duties
API --> ReportService: Generates Reports
API --> NotificationService: Sends Notifications

AuthService --> MongoDB: User Data
ExpenseService --> MongoDB: Expense Data
MealService --> MongoDB: Meal Data
DutyService --> MongoDB: Duty Data
ReportService --> MongoDB: Report Data
NotificationService --> Email/SMS Gateway: Sends Notifications

@enduml
```

#### Components

1. **Authentication Service**
   - **Technology:** JWT for authentication.
   - **Endpoints:**
     - `POST /api/auth/register`: Register new users.
     - `POST /api/auth/login`: Login and obtain a JWT.
     - `GET /api/auth/profile`: Get user profile details.

2. **Expense Service**
   - **Endpoints:**
     - `POST /api/expenses`: Add a new expense.
     - `GET /api/expenses`: List all expenses.
     - `PUT /api/expenses/:id`: Update an expense.
     - `DELETE /api/expenses/:id`: Delete an expense.
     - `GET /api/expenses/report`: Generate expense reports.

3. **Meal Service**
   - **Endpoints:**
     - `POST /api/meals`: Add meal consumption record.
     - `GET /api/meals`: List all meal records.
     - `PUT /api/meals/:id`: Update meal record.
     - `DELETE /api/meals/:id`: Delete meal record.
     - `GET /api/meals/report`: Generate meal reports.

4. **Duty Service**
   - **Endpoints:**
     - `POST /api/duties`: Assign a market duty.
     - `GET /api/duties`: List all duties.
     - `PUT /api/duties/:id`: Update a duty.
     - `DELETE /api/duties/:id`: Delete a duty.

5. **Report Service**
   - **Endpoints:**
     - `GET /api/reports/expenses`: Generate and export expense reports.
     - `GET /api/reports/meals`: Generate and export meal reports.

6. **Notification Service**
   - **Endpoints:**
     - `POST /api/notifications/email`: Send email notifications.
     - `POST /api/notifications/sms`: Send SMS notifications.

#### Database Schema

The MongoDB schema will include collections for users, expenses, meals, duties, and notifications.

```json
{
  "User": {
    "username": "String",
    "passwordHash": "String",
    "email": "String",
    "role": "String",
    "language": "String"
  },
  "Expense": {
    "description": "String",
    "amount": "Number",
    "category": "String",
    "date": "Date",
    "sharedBy": ["UserId"]
  },
  "Meal": {
    "date": "Date",
    "consumption": [{
      "userId": "UserId",
      "meals": "Number"
    }]
  },
  "Duty": {
    "assignedTo": "UserId",
    "date": "Date",
    "dutyDetails": "String"
  },
  "Notification": {
    "userId": "UserId",
    "message": "String",
    "type": "String",
    "status": "String",
    "date": "Date"
  }
}
```

### Implementation

The implementation of the Bachelor Home Management System (BHMS) backend will be divided into several steps to ensure a systematic and organized development process.

#### Step 1: Project Setup
1. **Initialize Project:**
   - Create a new Node.js project using `npm init`.
   - Set up the directory structure.
   - Install necessary dependencies: Express.js, Mongoose, JWT, Nodemailer (for notifications), etc.
   - Set up Git for version control.

2. **Configure Environment Variables:**
   - Create a `.env` file to store environment-specific configurations (e.g., database URI, JWT secret, email service credentials).

#### Step 2: Database Setup
1. **MongoDB Configuration:**
   - Set up MongoDB Atlas for a cloud-hosted database solution.
   - Create a new database for BHMS.

2. **Define Schemas:**
   - Define Mongoose schemas and models for User, Expense, Meal, Duty, and Notification.

#### Step 3: Authentication Service
1. **User Registration and Login:**
   - Implement user registration (`POST /api/auth/register`) and login (`POST /api/auth/login`).
   - Use bcrypt to hash passwords.
   - Generate JWT tokens for authenticated users.

2. **User Profile Management:**
   - Implement profile retrieval endpoint (`GET /api/auth/profile`).

#### Step 4: Expense Management
1. **CRUD Operations:**
   - Implement endpoints to add (`POST /api/expenses`), list (`GET /api/expenses`), update (`PUT /api/expenses/:id`), and delete expenses (`DELETE /api/expenses/:id`).

2. **Expense Reports:**
   - Implement report generation endpoint (`GET /api/expenses/report`).

#### Step 5: Meal Management
1. **CRUD Operations:**
   - Implement endpoints to add (`POST /api/meals`), list (`GET /api/meals`), update (`PUT /api/meals/:id`), and delete meal records (`DELETE /api/meals/:id`).

2. **Meal Reports:**
   - Implement report generation endpoint (`GET /api/meals/report`).

#### Step 6: Market Duty Management
1. **CRUD Operations:**
   - Implement endpoints to assign (`POST /api/duties`), list (`GET /api/duties`), update (`PUT /api/duties/:id`), and delete duties (`DELETE /api/duties/:id`).

#### Step 7: Notification Service
1. **Email and SMS Notifications:**
   - Implement endpoints to send email (`POST /api/notifications/email`) and SMS notifications (`POST /api/notifications/sms`).
   - Integrate with email and SMS gateways (e.g., Nodemailer for email, Twilio for SMS).

2. **In-App Notifications:**
   - Use WebSockets to implement real-time notifications within the app.

#### Step 8: Testing
1. **Unit Testing:**
   - Write unit tests for each service using a testing framework like Mocha or Jest.

2. **Integration Testing:**
   - Test the integration between different services and MongoDB.

3. **API Testing:**
   - Use Postman to test all API endpoints and ensure they are working as expected.

#### Step 9: Deployment
1. **Backend Deployment:**
   - Deploy the backend on Heroku.
   - Set up environment variables on Heroku.

2. **Database Deployment:**
   - Ensure MongoDB Atlas is properly configured and connected to the backend.

#### Step 10: Post-Deployment
1. **Monitoring:**
   - Monitor the application using tools like Heroku's built-in monitoring, or third-party services like New Relic.

2. **Bug Fixes and Improvements:**
   - Collect user feedback and make necessary improvements.
   - Fix any bugs reported post-deployment.


Given the 15-day time constraint, we'll need to streamline the milestones and focus on delivering a minimal viable product (MVP) that covers the essential features. Here’s a revised milestones plan:

### Revised Milestones

#### Milestone 1: Project Setup and Initial Configuration
- **Duration:** 1 day
- **Tasks:**
  - Initialize Node.js project.
  - Set up directory structure.
  - Install dependencies.
  - Configure environment variables.
  - Set up Git for version control.

#### Milestone 2: Database Setup and Schema Definition
- **Duration:** 2 days
- **Tasks:**
  - Set up MongoDB Atlas.
  - Create BHMS database.
  - Define and implement Mongoose schemas for User, Expense, Meal, and Duty.

#### Milestone 3: Authentication Service
- **Duration:** 2 days
- **Tasks:**
  - Implement user registration and login endpoints.
  - Implement user profile retrieval endpoint.
  - Test authentication functionality.

#### Milestone 4: Core Expense Management Service
- **Duration:** 3 days
- **Tasks:**
  - Implement CRUD endpoints for expenses.
  - Implement expense report generation endpoint.
  - Test expense management functionality.

#### Milestone 5: Core Meal Management Service
- **Duration:** 2 days
- **Tasks:**
  - Implement CRUD endpoints for meals.
  - Implement meal report generation endpoint.
  - Test meal management functionality.

#### Milestone 6: Core Market Duty Management Service
- **Duration:** 2 days
- **Tasks:**
  - Implement CRUD endpoints for duties.
  - Test duty management functionality.

#### Milestone 7: Basic Notification Service
- **Duration:** 2 days
- **Tasks:**
  - Implement email notifications.
  - Integrate with an email gateway (e.g., Nodemailer).
  - Test notification functionality.

#### Milestone 8: Testing and Deployment
- **Duration:** 3 days
- **Tasks:**
  - Write essential unit tests.
  - Conduct integration and API testing using Postman.
  - Deploy backend on Heroku.
  - Configure environment variables on Heroku.
  - Ensure MongoDB Atlas connectivity.

### Summary Timeline
1. **Day 1:** Project Setup and Initial Configuration
2. **Days 2-3:** Database Setup and Schema Definition
3. **Days 4-5:** Authentication Service
4. **Days 6-8:** Core Expense Management Service
5. **Days 9-10:** Core Meal Management Service
6. **Days 11-12:** Core Market Duty Management Service
7. **Days 13-14:** Basic Notification Service
8. **Days 15:** Testing and Deployment

By focusing on the core functionalities and ensuring they are thoroughly tested, I can deliver an MVP within the 15-day timeline. 


