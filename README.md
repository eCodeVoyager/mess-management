### Bachelor Home Management System (BHMS) Requirements and Implementation Plan

#### System/App Workflow

1. **User Management**
    - **New Member Registration**: Only an admin can create a user account for a new member.
    - **Profile Update**: New members must update their profile information, including:
        - Security documents (e.g., NID, Birth Certificate)
        - Emergency contact information
    - **Public Profile**: Users can view a list of current users and visit their profiles for any public information.

2. **Meal Management**
    - **Daily Meal Update**: Users can update their meal preferences for up to the next 10 days.
    - **Meal Timing**: Users must update their meal preferences at least 2 hours before meal time.
    - **Meal Calculation**: At the end of the month, the total meal expenses are divided by the total number of meals to determine the meal rate. This data is shown in a table format.

3. **Expense Management**
    - **Market Duty Expenses**: Only the assigned market duty member can add expenses for the day.
    - **Expense Approval**: Other members can add expenses, but these must be approved by an admin.
    - **Monthly Expense Calculation**: At the end of the month, calculate the meal rate and the total meal expense for each member, auto-generate this data, and send notifications to pay.

4. **Market Duty Roster**
    - **Auto-Assignment**: The system auto-assigns market duty to users based on an algorithm that considers total members and their availability.
    - **Availability**: Members can add their free and busy days to their profile so the market duty roster can assign duties accordingly.
    - **Duty Withdrawal**: Members can withdraw from market duty, but only if another member accepts the duty. Otherwise, they must fulfill their duty.
    - **Notifications**: The system sends notifications to users about their market duty assignments.

5. **Reports Generation**
    - **Daily, Weekly, Monthly Reports**: The system auto-generates reports on expenses and meal consumption.

6. **Notifications**
    - **System Notifications**: The entire system relies heavily on notifications to keep users informed about duties, pending payments, and other important updates.

### Requirements V2

**Must Have**

1. **User Management**
    - Admin-only user account creation.
    - User profile management with detailed information (security documents, emergency contact).
    - Public user profiles.

2. **Expense Management**
    - RESTful APIs to track food market expenses and other communal expenses.
    - Divide expenses among all residents.
    - Generate detailed expense reports via API.

3. **Meal Management**
    - RESTful APIs to track daily meal consumption.
    - Calculate meal prices based on market expenses.
    - Provide a monthly meal price breakdown via API.

4. **Market Duty Roster**
    - RESTful APIs to assign market duty to residents.
    - Create a roster to define who is responsible for purchasing groceries.

5. **Reports Generation**
    - Generate monthly and weekly reports of expenses and meal consumption.
    - Export reports in PDF format via API.

6. **Notifications**
    - APIs to notify users about their duties, pending payments, and other important updates via email/SMS.

**Should Have**

1. **In-App Notifications**
    - Real-time notifications within the app using WebSockets.

2. **Database Management**
    - Efficient MongoDB schema design to handle large datasets.
    - Indexing for faster query performance.

**Could Have**

1. **Additional Language Support**
    - APIs to support more languages based on user demand.

2. **Mobile Responsiveness**
    - Ensure APIs are optimized for mobile app consumption.

**Won’t Have**

1. **Advanced Analytics**
    - Complex data analysis and forecasting features will not be included in the initial version.

---

### Method

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
    - **Technology**: JWT for authentication.
    - **Endpoints**:
        - `POST /api/auth/register`: Register new users.
        - `POST /api/auth/login`: Login and obtain a JWT.
        - `GET /api/auth/profile`: Get user profile details.

2. **Expense Service**
    - **Endpoints**:
        - `POST /api/expenses`: Add a new expense.
        - `GET /api/expenses`: List all expenses.
        - `PUT /api/expenses/:id`: Update an expense.
        - `DELETE /api/expenses/:id`: Delete an expense.
        - `GET /api/expenses/report`: Generate expense reports.

3. **Meal Service**
    - **Endpoints**:
        - `POST /api/meals`: Add meal consumption record.
        - `GET /api/meals`: List all meal records.
        - `PUT /api/meals/:id`: Update meal record.
        - `DELETE /api/meals/:id`: Delete meal record.
        - `GET /api/meals/report`: Generate meal reports.

4. **Duty Service**
    - **Endpoints**:
        - `POST /api/duties`: Assign a market duty.
        - `GET /api/duties`: List all duties.
        - `PUT /api/duties/:id`: Update a duty.
        - `DELETE /api/duties/:id`: Delete a duty.

5. **Report Service**
    - **Endpoints**:
        - `GET /api/reports/expenses`: Generate and export expense reports.
        - `GET /api/reports/meals`: Generate and export meal reports.

6. **Notification Service**
    - **Endpoints**:
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
    "role": "String", // e.g., "admin", "member"
    "language": "String",
    "NID": "String",
    "birthCertificate": "String",
    "firstName": "String",
    "lastName": "String",
    "dateOfBirth": "Date",
    "phoneNumber": "String",
    "address": {
      "street": "String",
      "city": "String",
      "state": "String",
      "zipCode": "String",
      "country": "String"
    },
    "emergencyContact": {
      "name": "String",
      "relationship": "String",
      "phoneNumber": "String",
      "email": "String"
    },
    "profilePicture": "String", // URL to the profile picture
    "bio": "String",
    "socialLinks": {
      "facebook": "String",
      "twitter": "String",
      "linkedIn": "String",
      "instagram": "String"
    },
    "availability": {
      "freeDays": ["Date"],
      "busyDays": ["Date"]
    },
    "createdAt": "Date",
    "updatedAt": "Date"
  },
  "Expense": {
    "description": "String",
    "amount": "Number",
    "category": "String", // e.g., "food", "utility", "miscellaneous"
    "date": "Date",
    "paidBy": "UserId", // User who paid the expense
    "sharedBy": ["UserId"], // Users sharing the expense
    "approved": "Boolean",
    "createdAt": "Date",
    "updatedAt": "Date"
  },
  "Meal": {
    "date": "Date",
    "consumption": [{
      "userId": "UserId",
      "meals": "Number",
      "mealType": "String" // e.g., "breakfast", "lunch", "dinner"
    }],
    "createdAt": "Date",
    "updatedAt": "Date"
  },
  "Duty": {
    "assignedTo": "UserId",
    "date": "Date",
    "dutyDetails": "String",
    "status": "String", // e.g., "assigned", "completed", "pending"
    "notes": "String",
    "createdAt": "Date",
    "updatedAt": "Date"
  },
  "Notification": {
    "userId": "UserId",
    "message": "String",
    "type": "String", // e.g., "info", "warning", "alert"
    "status": "String", // e.g., "unread", "read"
    "date": "Date",
    "createdAt": "Date",
    "updatedAt": "Date"
  },
  "Report": {
    "type": "String", // e.g., "daily", "weekly", "monthly"
    "dateRange": {
      "start": "Date",
      "end": "Date"
    },
    "generatedBy": "UserId", // User who generated the report
    "content": "String", // Path to the report file or document content
    "createdAt": "Date",
    "updatedAt": "Date"
  }
}

```

---

### Implementation Plan

#### Step 1: Project Setup
- Initialize Project:
    - Create a new Node.js project using `npm init`.
    - Set up the directory structure.
    - Install necessary dependencies: Express.js, Mongoose, JWT, Nodemailer (for notifications), etc.
    - Set up Git for version control.

- Configure Environment Variables:
    - Create a `.env` file to store environment-specific configurations (e.g., database URI, JWT secret, email service credentials).

#### Step 2: Database Setup
- MongoDB Configuration:
    - Set up MongoDB Atlas for a cloud-hosted database solution.
    - Create a new database for BHMS.

- Define Schemas:
    - Define Mongoose schemas and models for User, Expense, Meal, Duty, and Notification.

#### Step 3: Authentication Service
- User Registration and Login:
    - Implement user registration (`POST /api/auth/register`) and login (`POST /api/auth/login`).
    - Use bcrypt to hash passwords.
    - Generate JWT tokens for authenticated users.

- User Profile Management:
    - Implement profile retrieval endpoint (`GET /api/auth/profile`).

#### Step 4: Expense Management
- CRUD Operations:
    - Implement endpoints to add (`POST /api/expenses`), list (`GET /api/expenses`), update (`PUT /api/expenses/:id`), and delete expenses (`DELETE /api/expenses/:id`).
    - Ensure that non-market duty expenses require admin approval.

- Expense Reports:
    - Implement report generation endpoint (`GET /api/expenses/report`).

#### Step 5: Meal Management
- CRUD Operations:
    - Implement endpoints to add (`POST /api/meals`), list (`GET /api/meals`), update (`PUT /api/meals/:id`), and delete meal records

 (`DELETE /api/meals/:id`).

- Meal Reports:
    - Implement report generation endpoint (`GET /api/meals/report`).

#### Step 6: Market Duty Management
- CRUD Operations:
    - Implement endpoints to assign (`POST /api/duties`), list (`GET /api/duties`), update (`PUT /api/duties/:id`), and delete duties (`DELETE /api/duties/:id`).

#### Step 7: Notification Service
- Email and SMS Notifications:
    - Implement endpoints to send email (`POST /api/notifications/email`) and SMS notifications (`POST /api/notifications/sms`).
    - Integrate with email and SMS gateways (e.g., Nodemailer for email, Twilio for SMS).

- In-App Notifications:
    - Use WebSockets to implement real-time notifications within the app.

#### Step 8: Testing
- Unit Testing:
    - Write unit tests for each service using a testing framework like Mocha or Jest.

- Integration Testing:
    - Test the integration between different services and MongoDB.

- API Testing:
    - Use Postman to test all API endpoints and ensure they are working as expected.

#### Step 9: Deployment
- Backend Deployment:
    - Deploy the backend on Heroku.
    - Set up environment variables on Heroku.

- Database Deployment:
    - Ensure MongoDB Atlas is properly configured and connected to the backend.

#### Step 10: Post-Deployment
- Monitoring:
    - Monitor the application using tools like Heroku's built-in monitoring, or third-party services like New Relic.

- Bug Fixes and Improvements:
    - Collect user feedback and make necessary improvements.
    - Fix any bugs reported post-deployment.

### Revised Milestones

**Milestone 1: Project Setup and Initial Configuration**
- Duration: 1 day
- Tasks:
    - Initialize Node.js project.
    - Set up directory structure.
    - Install dependencies.
    - Configure environment variables.
    - Set up Git for version control.

**Milestone 2: Database Setup and Schema Definition**
- Duration: 2 days
- Tasks:
    - Set up MongoDB Atlas.
    - Create BHMS database.
    - Define and implement Mongoose schemas for User, Expense, Meal, and Duty.

**Milestone 3: Authentication Service**
- Duration: 2 days
- Tasks:
    - Implement user registration and login endpoints.
    - Implement user profile retrieval endpoint.
    - Test authentication functionality.

**Milestone 4: Core Expense Management Service**
- Duration: 3 days
- Tasks:
    - Implement CRUD endpoints for expenses.
    - Implement expense report generation endpoint.
    - Test expense management functionality.

**Milestone 5: Core Meal Management Service**
- Duration: 2 days
- Tasks:
    - Implement CRUD endpoints for meals.
    - Implement meal report generation endpoint.
    - Test meal management functionality.

**Milestone 6: Core Market Duty Management Service**
- Duration: 2 days
- Tasks:
    - Implement CRUD endpoints for duties.
    - Test duty management functionality.

**Milestone 7: Basic Notification Service**
- Duration: 2 days
- Tasks:
    - Implement email notifications.
    - Integrate with an email gateway (e.g., Nodemailer).
    - Test notification functionality.

**Milestone 8: Testing and Deployment**
- Duration: 3 days
- Tasks:
    - Write essential unit tests.
    - Conduct integration and API testing using Postman.
    - Deploy backend on Heroku.
    - Configure environment variables on Heroku.
    - Ensure MongoDB Atlas connectivity.

### Summary Timeline

- **Day 1**: Project Setup and Initial Configuration
- **Days 2-3**: Database Setup and Schema Definition
- **Days 4-5**: Authentication Service
- **Days 6-8**: Core Expense Management Service
- **Days 9-10**: Core Meal Management Service
- **Days 11-12**: Core Market Duty Management Service
- **Days 13-14**: Basic Notification Service
- **Day 15**: Testing and Deployment

By focusing on the core functionalities and ensuring they are thoroughly tested, the MVP can be delivered within the 15-day timeline.
