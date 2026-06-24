# Simple Android Wellness App — Product & API Document

## 1. Project Overview

### Project Name

**SimpleWell**

### Project Type

A simplified Android version of the built-in iOS Health/Wellness app.

### Main Idea

SimpleWell allows users to record their daily wellness data, such as sleep, mood, water intake, steps, exercise time, and personal notes. The app then uses a Python-based AI service to generate simple daily wellness suggestions.

This app is not a medical diagnosis system. It is only a personal wellness tracking and suggestion app.

------

## 2. Technology Stack

### Android Frontend

- Kotlin
- Android Studio
- Jetpack ViewModel
- Retrofit
- Coroutines
- SharedPreferences or DataStore for JWT storage

### Backend

- Java 17
- Spring Boot 3
- Spring Security
- JWT authentication
- Spring Data JPA
- PostgreSQL or MySQL
- RESTful APIs

### AI Service

- Python
- FastAPI
- Prompt engineering
- Optional LLM API integration
- Optional simple RAG knowledge base for wellness tips

------

## 3. Simplified MVP Scope

### Included Features

1. User registration and login
2. JWT-secured API access
3. Create daily wellness log
4. View wellness logs by date
5. View weekly wellness summary
6. Generate AI wellness advice
7. Android frontend consumes Spring Boot APIs
8. Spring Boot backend calls Python FastAPI AI service

### Not Included in MVP

1. Real Apple Health or Google Fit integration
2. Wearable device integration
3. Doctor/medical diagnosis features
4. Push notifications
5. Complex charts
6. Social sharing
7. Payment features

------

## 4. System Architecture

```text
Android Kotlin App
        |
        | HTTPS + JWT
        v
Spring Boot Backend
        |
        | Internal REST API
        v
Python FastAPI AI Service
        |
        | Optional LLM API
        v
AI-generated wellness advice
```

### Responsibility of Each Part

#### Android App

The Android app is responsible for:

- Displaying login and register screens
- Letting users create wellness logs
- Showing daily and weekly wellness records
- Sending requests to Spring Boot backend
- Storing JWT token locally
- Displaying AI-generated advice

#### Spring Boot Backend

The backend is responsible for:

- User authentication
- JWT generation and validation
- CRUD operations for wellness logs
- Storing data in the database
- Calling the Python AI service
- Returning structured responses to the Android app

#### Python AI Service

The Python AI service is responsible for:

- Receiving user wellness data
- Building a prompt
- Generating simple personalized advice
- Returning AI advice to Spring Boot

------

## 5. Core User Stories

### User Story 1: Register

As a new user, I want to create an account so that I can save my wellness records.

### User Story 2: Login

As a user, I want to log in securely so that only I can access my wellness data.

### User Story 3: Create Daily Wellness Log

As a user, I want to record my daily sleep, mood, water intake, steps, exercise, and notes.

### User Story 4: View Wellness History

As a user, I want to view my previous wellness logs so that I can understand my habits.

### User Story 5: Get AI Wellness Advice

As a user, I want the app to generate simple AI advice based on my recent wellness data.

------

## 6. Main Screens in Android App

### 6.1 Login Screen

Fields:

- Email
- Password

Actions:

- Login
- Navigate to Register screen

### 6.2 Register Screen

Fields:

- Username
- Email
- Password

Actions:

- Register
- Navigate back to Login screen

### 6.3 Home Screen

Displays:

- Today’s wellness status
- Button to add today’s log
- Button to view AI advice
- Button to view history

### 6.4 Add Wellness Log Screen

Fields:

- Sleep hours
- Mood score
- Water intake
- Steps
- Exercise minutes
- Notes

### 6.5 History Screen

Displays:

- List of wellness logs
- Date
- Sleep hours
- Mood score
- Steps
- Summary note

### 6.6 AI Advice Screen

Displays:

- AI-generated daily advice
- Key wellness observations
- Suggested actions

------

## 7. Database Design

## 7.1 User Table

Table name: `users`

| Field        | Type     | Nullable | Description           |
| ------------ | -------- | -------- | --------------------- |
| id           | Long     | No       | User ID               |
| username     | String   | No       | Display name          |
| email        | String   | No       | Login email           |
| passwordHash | String   | No       | Encrypted password    |
| createdAt    | Datetime | No       | Account creation time |
| updatedAt    | Datetime | Yes      | Last update time      |

------

## 7.2 Wellness Log Table

Table name: `wellness_logs`

| Field           | Type     | Nullable | Description             |
| --------------- | -------- | -------- | ----------------------- |
| id              | Long     | No       | Log ID                  |
| userId          | Long     | No       | Owner user ID           |
| logDate         | Date     | No       | Wellness record date    |
| sleepHours      | Decimal  | Yes      | Sleep duration          |
| moodScore       | Integer  | Yes      | Mood score from 1 to 5  |
| waterCups       | Integer  | Yes      | Number of cups of water |
| steps           | Integer  | Yes      | Daily steps             |
| exerciseMinutes | Integer  | Yes      | Exercise time           |
| note            | String   | Yes      | User note               |
| createdAt       | Datetime | No       | Creation time           |
| updatedAt       | Datetime | Yes      | Update time             |

------

## 7.3 AI Advice Table

Table name: `ai_advice`

| Field      | Type     | Nullable | Description         |
| ---------- | -------- | -------- | ------------------- |
| id         | Long     | No       | Advice ID           |
| userId     | Long     | No       | User ID             |
| adviceDate | Date     | No       | Advice date         |
| adviceText | Text     | No       | AI-generated advice |
| createdAt  | Datetime | No       | Creation time       |

------

# 8. API Contract

## 8.1 General Rules

### Base URL

```text
http://localhost:8080/api
```

### Authentication

Protected APIs require JWT.

Request header:

```http
Authorization: Bearer <jwt_token>
```

### Common Success Response Format

```json
{
  "success": true,
  "message": "Success",
  "data": {}
}
```

### Common Error Response Format

```json
{
  "success": false,
  "message": "Error message",
  "errorCode": "ERROR_CODE"
}
```

------

# 9. Auth APIs

## 9.1 Register

### Endpoint

```http
POST /api/auth/register
```

### Description

Create a new user account.

### Request Body

```json
{
  "username": "Dadao",
  "email": "dadao@example.com",
  "password": "Password123"
}
```

### Response Body

```json
{
  "success": true,
  "message": "User registered successfully",
  "data": {
    "id": 1,
    "username": "Dadao",
    "email": "dadao@example.com"
  }
}
```

------

## 9.2 Login

### Endpoint

```http
POST /api/auth/login
```

### Description

Login and receive a JWT token.

### Request Body

```json
{
  "email": "dadao@example.com",
  "password": "Password123"
}
```

### Response Body

```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "token": "jwt_token_here",
    "user": {
      "id": 1,
      "username": "Dadao",
      "email": "dadao@example.com"
    }
  }
}
```

------

# 10. User APIs

## 10.1 Get Current User

### Endpoint

```http
GET /api/users/me
```

### Authentication

Required.

### Response Body

```json
{
  "success": true,
  "message": "Success",
  "data": {
    "id": 1,
    "username": "Dadao",
    "email": "dadao@example.com"
  }
}
```

------

# 11. Wellness Log APIs

## 11.1 Create Wellness Log

### Endpoint

```http
POST /api/wellness-logs
```

### Authentication

Required.

### Description

Create a wellness log for a specific date.

### Request Body

```json
{
  "logDate": "2026-06-24",
  "sleepHours": 7.5,
  "moodScore": 4,
  "waterCups": 6,
  "steps": 8000,
  "exerciseMinutes": 30,
  "note": "Felt good today, but a little tired in the afternoon."
}
```

### Response Body

```json
{
  "success": true,
  "message": "Wellness log created successfully",
  "data": {
    "id": 101,
    "logDate": "2026-06-24",
    "sleepHours": 7.5,
    "moodScore": 4,
    "waterCups": 6,
    "steps": 8000,
    "exerciseMinutes": 30,
    "note": "Felt good today, but a little tired in the afternoon.",
    "createdAt": "2026-06-24T10:30:00"
  }
}
```

------

## 11.2 Get Wellness Logs

### Endpoint

```http
GET /api/wellness-logs
```

### Authentication

Required.

### Query Parameters

| Parameter | Type | Required | Example    | Description |
| --------- | ---- | -------- | ---------- | ----------- |
| startDate | Date | No       | 2026-06-01 | Start date  |
| endDate   | Date | No       | 2026-06-24 | End date    |

### Example Request

```http
GET /api/wellness-logs?startDate=2026-06-01&endDate=2026-06-24
```

### Response Body

```json
{
  "success": true,
  "message": "Success",
  "data": [
    {
      "id": 101,
      "logDate": "2026-06-24",
      "sleepHours": 7.5,
      "moodScore": 4,
      "waterCups": 6,
      "steps": 8000,
      "exerciseMinutes": 30,
      "note": "Felt good today."
    }
  ]
}
```

------

## 11.3 Get Wellness Log by Date

### Endpoint

```http
GET /api/wellness-logs/date/{logDate}
```

### Authentication

Required.

### Example Request

```http
GET /api/wellness-logs/date/2026-06-24
```

### Response Body

```json
{
  "success": true,
  "message": "Success",
  "data": {
    "id": 101,
    "logDate": "2026-06-24",
    "sleepHours": 7.5,
    "moodScore": 4,
    "waterCups": 6,
    "steps": 8000,
    "exerciseMinutes": 30,
    "note": "Felt good today."
  }
}
```

------

## 11.4 Update Wellness Log

### Endpoint

```http
PUT /api/wellness-logs/{id}
```

### Authentication

Required.

### Request Body

```json
{
  "sleepHours": 8.0,
  "moodScore": 5,
  "waterCups": 7,
  "steps": 9000,
  "exerciseMinutes": 35,
  "note": "Updated my record."
}
```

### Response Body

```json
{
  "success": true,
  "message": "Wellness log updated successfully",
  "data": {
    "id": 101,
    "logDate": "2026-06-24",
    "sleepHours": 8.0,
    "moodScore": 5,
    "waterCups": 7,
    "steps": 9000,
    "exerciseMinutes": 35,
    "note": "Updated my record."
  }
}
```

------

## 11.5 Delete Wellness Log

### Endpoint

```http
DELETE /api/wellness-logs/{id}
```

### Authentication

Required.

### Response Body

```json
{
  "success": true,
  "message": "Wellness log deleted successfully",
  "data": null
}
```

------

# 12. Summary APIs

## 12.1 Get Weekly Summary

### Endpoint

```http
GET /api/wellness-summary/weekly
```

### Authentication

Required.

### Query Parameters

| Parameter | Type | Required | Example    |
| --------- | ---- | -------- | ---------- |
| startDate | Date | No       | 2026-06-17 |
| endDate   | Date | No       | 2026-06-24 |

### Response Body

```json
{
  "success": true,
  "message": "Success",
  "data": {
    "averageSleepHours": 7.2,
    "averageMoodScore": 4.1,
    "averageWaterCups": 6.5,
    "totalSteps": 56000,
    "totalExerciseMinutes": 180,
    "summary": "Your sleep and exercise were stable this week."
  }
}
```

------

# 13. AI Advice APIs

## 13.1 Generate AI Advice

### Endpoint

```http
POST /api/ai/advice
```

### Authentication

Required.

### Description

Spring Boot collects recent wellness logs and sends them to the Python AI service. The AI service returns personalized advice.

### Request Body

```json
{
  "startDate": "2026-06-17",
  "endDate": "2026-06-24"
}
```

### Response Body

```json
{
  "success": true,
  "message": "AI advice generated successfully",
  "data": {
    "adviceDate": "2026-06-24",
    "adviceText": "You slept around 7 hours per day this week, which is stable. Your exercise time is good. Try to drink slightly more water and take short breaks when you feel tired in the afternoon."
  }
}
```

------

## 13.2 Get Latest AI Advice

### Endpoint

```http
GET /api/ai/advice/latest
```

### Authentication

Required.

### Response Body

```json
{
  "success": true,
  "message": "Success",
  "data": {
    "adviceDate": "2026-06-24",
    "adviceText": "Try to maintain your sleep schedule and increase water intake."
  }
}
```

------

# 14. Internal Python AI Service API

This API is called by the Spring Boot backend, not directly by the Android app.

## 14.1 Generate Wellness Advice

### Base URL

```text
http://localhost:8000
```

### Endpoint

```http
POST /ai/wellness-advice
```

### Request Body

```json
{
  "userId": 1,
  "logs": [
    {
      "logDate": "2026-06-24",
      "sleepHours": 7.5,
      "moodScore": 4,
      "waterCups": 6,
      "steps": 8000,
      "exerciseMinutes": 30,
      "note": "Felt good today, but a little tired."
    }
  ]
}
```

### Response Body

```json
{
  "adviceText": "Your sleep and exercise are generally good. Your water intake is slightly low. Try to drink one or two more cups of water tomorrow and take a short walk after lunch."
}
```

------

# 15. AI Prompt Design

## System Prompt

```text
You are a simple wellness assistant.

You are not a doctor.
You must not provide medical diagnosis.
You should only give general lifestyle suggestions.

Analyze the user's recent wellness logs and give short, practical advice.

Return only valid JSON.
```

## User Prompt Example

```text
Here are the user's wellness logs:

Date: 2026-06-24
Sleep: 7.5 hours
Mood: 4/5
Water: 6 cups
Steps: 8000
Exercise: 30 minutes
Note: Felt good today, but a little tired.

Generate simple wellness advice.
```

## Expected AI Output

```json
{
  "adviceText": "Your sleep and exercise look stable. Your water intake is acceptable but could be slightly improved. Try to take a short break in the afternoon if you feel tired."
}
```

------

# 16. Android Frontend API Usage

## Retrofit API Interface Example

```kotlin
interface ApiService {

    @POST("api/auth/login")
    suspend fun login(
        @Body request: LoginRequest
    ): ApiResponse<LoginResponse>

    @POST("api/auth/register")
    suspend fun register(
        @Body request: RegisterRequest
    ): ApiResponse<UserResponse>

    @GET("api/users/me")
    suspend fun getCurrentUser(): ApiResponse<UserResponse>

    @POST("api/wellness-logs")
    suspend fun createWellnessLog(
        @Body request: WellnessLogRequest
    ): ApiResponse<WellnessLogResponse>

    @GET("api/wellness-logs")
    suspend fun getWellnessLogs(
        @Query("startDate") startDate: String?,
        @Query("endDate") endDate: String?
    ): ApiResponse<List<WellnessLogResponse>>

    @GET("api/wellness-summary/weekly")
    suspend fun getWeeklySummary(
        @Query("startDate") startDate: String?,
        @Query("endDate") endDate: String?
    ): ApiResponse<WeeklySummaryResponse>

    @POST("api/ai/advice")
    suspend fun generateAiAdvice(
        @Body request: AiAdviceRequest
    ): ApiResponse<AiAdviceResponse>

    @GET("api/ai/advice/latest")
    suspend fun getLatestAiAdvice(): ApiResponse<AiAdviceResponse>
}
```

------

# 17. Kotlin Data Classes

```kotlin
data class ApiResponse<T>(
    val success: Boolean,
    val message: String,
    val data: T?
)

data class RegisterRequest(
    val username: String,
    val email: String,
    val password: String
)

data class LoginRequest(
    val email: String,
    val password: String
)

data class LoginResponse(
    val token: String,
    val user: UserResponse
)

data class UserResponse(
    val id: Long,
    val username: String,
    val email: String
)

data class WellnessLogRequest(
    val logDate: String,
    val sleepHours: Double?,
    val moodScore: Int?,
    val waterCups: Int?,
    val steps: Int?,
    val exerciseMinutes: Int?,
    val note: String?
)

data class WellnessLogResponse(
    val id: Long,
    val logDate: String,
    val sleepHours: Double?,
    val moodScore: Int?,
    val waterCups: Int?,
    val steps: Int?,
    val exerciseMinutes: Int?,
    val note: String?
)

data class WeeklySummaryResponse(
    val averageSleepHours: Double,
    val averageMoodScore: Double,
    val averageWaterCups: Double,
    val totalSteps: Int,
    val totalExerciseMinutes: Int,
    val summary: String
)

data class AiAdviceRequest(
    val startDate: String,
    val endDate: String
)

data class AiAdviceResponse(
    val adviceDate: String,
    val adviceText: String
)
```

------

# 18. JWT Flow

## Login Flow

```text
1. User enters email and password in Android app.
2. Android sends POST /api/auth/login.
3. Spring Boot validates the user.
4. Spring Boot returns JWT token.
5. Android stores the token locally.
6. Android adds the token to future API requests.
```

## Protected API Flow

```text
1. Android sends request with Authorization header.
2. Spring Security JWT filter validates token.
3. If token is valid, request continues.
4. If token is invalid, backend returns 401 Unauthorized.
```

------

# 19. Backend Package Structure

```text
src/main/java/com/example/simplewell
├── auth
│   ├── AuthController.java
│   ├── AuthService.java
│   ├── JwtService.java
│   └── JwtAuthenticationFilter.java
├── user
│   ├── User.java
│   ├── UserRepository.java
│   ├── UserController.java
│   └── UserService.java
├── wellness
│   ├── WellnessLog.java
│   ├── WellnessLogRepository.java
│   ├── WellnessLogController.java
│   ├── WellnessLogService.java
│   └── dto
├── summary
│   ├── WellnessSummaryController.java
│   └── WellnessSummaryService.java
├── ai
│   ├── AiAdvice.java
│   ├── AiAdviceRepository.java
│   ├── AiController.java
│   ├── AiService.java
│   └── AiClient.java
├── common
│   ├── ApiResponse.java
│   └── GlobalExceptionHandler.java
└── config
    ├── SecurityConfig.java
    └── CorsConfig.java
```

------

# 20. Python FastAPI Project Structure

```text
ai-service
├── main.py
├── schemas.py
├── prompt_builder.py
├── ai_service.py
└── requirements.txt
```

------

# 21. Python FastAPI Example

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import List, Optional

app = FastAPI()

class WellnessLog(BaseModel):
    logDate: str
    sleepHours: Optional[float] = None
    moodScore: Optional[int] = None
    waterCups: Optional[int] = None
    steps: Optional[int] = None
    exerciseMinutes: Optional[int] = None
    note: Optional[str] = None

class WellnessAdviceRequest(BaseModel):
    userId: int
    logs: List[WellnessLog]

class WellnessAdviceResponse(BaseModel):
    adviceText: str

@app.post("/ai/wellness-advice", response_model=WellnessAdviceResponse)
def generate_wellness_advice(request: WellnessAdviceRequest):
    if not request.logs:
        return WellnessAdviceResponse(
            adviceText="There is not enough wellness data yet. Try recording your sleep, mood, water intake, and exercise for a few days."
        )

    latest_log = request.logs[-1]

    advice = "Based on your recent wellness data, try to keep a stable sleep schedule, drink enough water, and do light exercise regularly."

    if latest_log.sleepHours is not None and latest_log.sleepHours < 6:
        advice += " Your sleep time seems low, so try to rest earlier tonight."

    if latest_log.waterCups is not None and latest_log.waterCups < 5:
        advice += " Your water intake seems low, so try to drink more water tomorrow."

    if latest_log.exerciseMinutes is not None and latest_log.exerciseMinutes < 15:
        advice += " You may also add a short walk or light exercise."

    return WellnessAdviceResponse(adviceText=advice)
```

------

# 22. Spring Boot Calling Python AI Service

## Example Flow

```text
Android App
  -> POST /api/ai/advice
Spring Boot
  -> Reads recent wellness logs from database
  -> Calls Python FastAPI POST /ai/wellness-advice
Python AI Service
  -> Returns adviceText
Spring Boot
  -> Saves adviceText into database
  -> Returns adviceText to Android app
```

------

# 23. Recommended Implementation Order

## Phase 1: Backend Foundation

1. Create Spring Boot project
2. Configure database
3. Create User entity
4. Implement register and login
5. Implement JWT security

## Phase 2: Wellness Log APIs

1. Create WellnessLog entity
2. Implement create log API
3. Implement get logs API
4. Implement update log API
5. Implement delete log API

## Phase 3: Android App

1. Create Android Kotlin project
2. Build Login screen
3. Build Register screen
4. Configure Retrofit
5. Store JWT token
6. Build Wellness Log screen
7. Build History screen

## Phase 4: Python AI Service

1. Create FastAPI project
2. Create `/ai/wellness-advice` endpoint
3. Test it through browser or Postman
4. Connect Spring Boot to FastAPI

## Phase 5: Final Integration

1. Android calls Spring Boot
2. Spring Boot calls Python AI service
3. Android displays AI advice
4. Test JWT-protected APIs
5. Prepare final demo

------

# 24. Minimum Demo Scenario

1. User registers an account.
2. User logs in.
3. Android app receives JWT token.
4. User creates a wellness log.
5. User views wellness history.
6. User clicks “Generate AI Advice”.
7. Backend calls Python AI service.
8. AI advice is returned and shown in Android app.

------

# 25. Final Project Description

SimpleWell is a simplified Android wellness tracking app inspired by the built-in iOS Health app. It allows users to securely record daily wellness data, including sleep, mood, water intake, steps, exercise time, and personal notes. The system uses a Kotlin Android frontend, a Spring Boot backend with JWT authentication, and a Python FastAPI-based AI service to generate simple personalized wellness advice. The project demonstrates secure mobile API integration, RESTful backend development, and basic LLM application integration.