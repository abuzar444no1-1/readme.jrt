# LMS API Documentation

**API Version:** v1  
**Base URL:** (not specified)  
**Authentication:** Bearer token required for all endpoints. Include `Authorization: Bearer <token>` header.

---

# Analytics

### Endpoint: GET /api/analytics/dashboard

**Description:** Fetch analytics dashboard data.

**Parameters:** None

**Responses**

**200: Success**

```json
{
  "totalStudents": 150,
  "totalCourses": 12,
  "averageAttendance": 85,
  "completionRate": 78
}
```

**500:** Error fetching analytics dashboard

---

# Assignment

### Endpoint: POST /api/assignments/create-assignment

**Description:** Create a new assignment.

**Request Body:** application/json, text/json, application/*+json

**Schema:** AssignmentDto

**Example**

```json
{
  "courseId": "3fa85f64-5717-4562-b3fc-2c963f66aghr",
  "title": "I AM",
  "description": "boby",
  "dueDate": "2026-03-11T16:30:02.385Z"
}
```

**Responses**

**200: Assignment created successfully**

```json
{
  "courseId": "3fa85f64-5717-4562-b3fc-2c963f66aghr",
  "title": "I AM",
  "description": "boby",
  "dueDate": "2026-03-11T16:30:02.385Z"
}
```

**400:** Invalid data  
**500:** Failed to create assignment

---

### Endpoint: POST /api/assignments/submit

**Description:** Submit an assignment (supports file upload).

**Request Body:** multipart/form-data

- assignmentId: string (UUID)
- file: file
- notes: string (optional)

**Responses**

**200: Submission successful**

```json
{
  "submissionId": "sub123",
  "message": "Assignment submitted"
}
```

**500:** Failed to submit

---

### Endpoint: POST /api/assignments/grade/{submissionId}

**Description:** Grade a submitted assignment.

**Path Parameters**

- submissionId: string (UUID) (required)

**Query Parameters**

- grade: integer (int32) (required)

**Responses**

**200: Graded successfully**

```json
{
  "message": "Grade recorded"
}
```

**500:** Failed to grade

---

### Endpoint: GET /api/assignments/course/{courseId}

**Description:** Get all assignments for a specific course.

**Path Parameters**

- courseId: string (UUID) (required)

**Responses**

**200: Array of assignments**

```json
[
  {
    "id": "b52ae38c-2279-4ded-b5d3-62bdee3eb290",
    "courseId": "af6095a0-b1d5-4cdd-4b64-08de4ad08ebb",
    "title": "STAT",
    "description": "please give clearly answer any question.",
    "dueDate": "2026-01-20T00:00:00",
    "createdBy": "e95be086-1b80-4776-8012-56402976d409"
  }
]
```

**500:** Failed to fetch assignments

---

### Endpoint: GET /api/assignments/student

**Description:** Get assignments for the current student.

**Responses**

**200: Array of assignments (with submission status)**

```json
[
  {
    "id": "b52ae38c-2279-4ded-b5d3-62bdee3eb290",
    "courseId": "af6095a0-b1d5-4cdd-4b64-08de4ad08ebb",
    "title": "STAT",
    "description": "please give clearly answer any question.",
    "dueDate": "2026-01-20T00:00:00",
    "createdBy": "e95be086-1b80-4776-8012-56402976d409",
    "submitted": true
  }
]
```

**500:** Failed to fetch assignments

---

# Attendance

### Endpoint: GET /api/Attendance/summary

**Description:** Fetch attendance summary for a given date.

**Query Parameters**

- date: string (date-time) – date in ISO format (e.g., 2025-03-10)

**Responses**

**200: Summary data**

```json
{
  "totalStudents": 30,
  "present": 25,
  "absent": 5,
  "percentage": 83.33
}
```

**500:** Failed to fetch summary

---

### Endpoint: GET /api/Attendance/students

**Description:** Fetch attendance list of students for a given date.

**Query Parameters**

- date: string (date-time) – date in ISO format (e.g., 2025-03-10)

**Responses**

**200: Array of students with attendance status**

```json
[
  {
    "studentId": "s1",
    "name": "John Doe",
    "status": "present"
  }
]
```

**500:** Failed to fetch students attendance

---

### Endpoint: POST /api/Attendance/mark

**Description:** Mark attendance for students.

**Request Body:** application/json, text/json, application/*+json

**Schema:** MarkAttendanceDto

**Example**

```json
{
  "studentId": 123,
  "date": "2025-03-10T00:00:00Z",
  "status": "present"
}
```

**Responses**

**200: Marked successfully**

```json
{
  "message": "Attendance recorded"
}
```

**500:** Failed to mark

---

# Auth

### Endpoint: POST /api/auth/generate-otp

**Description:** Generate OTP for registration or password reset.

**Request Body:** application/json, text/json, application/*+json – string (email)

**Example**

```json
"user@example.com"
```

**Responses**

**200: OTP sent**

```json
{
  "message": "OTP generated and sent"
}
```

**500:** Failed to generate OTP

---

### Endpoint: POST /api/auth/register

**Description:** Register a new user with OTP verification.

**Request Body:** application/json, text/json, application/*+json

**Schema:** RegisterWithOtpDto

**Example**

```json
{
  "registerDto": {
    "fullName": "John Doe",
    "email": "john@example.com",
    "password": "secret",
    "role": 1
  },
  "otp": "123456"
}
```

**Responses**

**200: Registration successful, returns token**

```json
{
  "token": "jwt_token",
  "user": {
    "id": "u123",
    "email": "john@example.com",
    "role": 1
  }
}
```

**400:** Invalid OTP or user exists  
**500:** Registration failed

---

### Endpoint: POST /api/auth/login

**Description:** Login with email and password.

**Request Body:** application/json, text/json, application/*+json

**Schema:** LoginDto

**Example**

```json
{
  "email": "john@example.com",
  "password": "secret"
}
```

**Responses**

**200: Login successful**

```json
{
  "token": "jwt_token",
  "user": {
    "id": "u123",
    "email": "john@example.com",
    "role": 1
  }
}
```

**401:** Invalid credentials  
**500:** Login failed
---

### Endpoint: POST /api/auth/forgot-password

**Description:** Send OTP for password reset.

**Request Body:** application/json, text/json, application/*+json – string (email)

**Example**

```json
"user@example.com"
```

**Responses**

**200: OTP sent for password reset**

```json
{
  "message": "Password reset OTP sent"
}
```

**400:** Email not found  
**500:** Failed to send OTP

---

### Endpoint: POST /api/auth/reset-password

**Description:** Reset password using OTP.

**Request Body:** application/json, text/json, application/*+json

**Schema:** ResetPasswordDto

**Example**

```json
{
  "email": "user@example.com",
  "otp": "123456",
  "newPassword": "NewStrongPassword123!"
}
```

**Responses**

**200: Password reset successful**

```json
{
  "message": "Password reset successfully"
}
```

**400:** Invalid OTP  
**500:** Password reset failed

---

# Courses

### Endpoint: POST /api/courses/create-course

**Description:** Create a new course.

**Request Body:** multipart/form-data

- Title: string  
- Subtitle: string  
- Description: string  
- Category: string  
- Level: integer  
- Price: number  
- Duration: number  
- Language: string  
- Thumbnail: file

**Responses**

**200: Course created successfully**

```json
{
  "id": "course123",
  "title": "Math 101",
  "category": "Mathematics"
}
```

**400:** Invalid course data  
**500:** Course creation failed

---

### Endpoint: GET /api/courses/all-courses

**Description:** Get all courses.

**Responses**

**200: Array of courses**

```json
[
  {
    "id": "course123",
    "title": "Math 101",
    "category": "Mathematics",
    "price": 20
  }
]
```

**500:** Failed to fetch courses

---

### Endpoint: GET /api/courses/{id}

**Description:** Get details of a specific course.

**Path Parameters**

- id: string (UUID)

**Responses**

**200: Course details**

```json
{
  "id": "course123",
  "title": "Math 101",
  "description": "Basic math concepts",
  "category": "Mathematics",
  "price": 20
}
```

**404:** Course not found  
**500:** Failed to fetch course

---

### Endpoint: PUT /api/courses/update-course/{id}

**Description:** Update an existing course.

**Path Parameters**

- id: string (UUID)

**Request Body:** application/json

```json
{
  "title": "Advanced Math",
  "description": "Updated description",
  "category": "Mathematics",
  "level": 2
}
```

**Responses**

**200: Course updated**

```json
{
  "message": "Course updated successfully"
}
```

**404:** Course not found  
**500:** Update failed

---

### Endpoint: DELETE /api/courses/delete-course/{id}

**Description:** Delete a course.

**Path Parameters**

- id: string (UUID)

**Responses**

**200: Course deleted**

```json
{
  "message": "Course deleted successfully"
}
```

**404:** Course not found  
**500:** Deletion failed

---

### Endpoint: POST /api/courses/{id}/enroll

**Description:** Enroll a student in a course.

**Path Parameters**

- id: string (UUID)

**Responses**

**200: Enrollment successful**

```json
{
  "message": "Student enrolled successfully"
}
```

**400:** Already enrolled  
**500:** Enrollment failed

---

### Endpoint: GET /api/courses/my-courses

**Description:** Get courses for the logged-in user.

**Responses**

**200: List of enrolled courses**

```json
[
  {
    "id": "course123",
    "title": "Math 101",
    "progress": 65
  }
]
```

**500:** Failed to fetch courses

---

### Endpoint: GET /api/courses/pending

**Description:** Get courses waiting for approval.

**Responses**

**200: Pending courses**

```json
[
  {
    "id": "course234",
    "title": "Physics Basics",
    "status": "pending"
  }
]
```

**500:** Failed to fetch pending courses

---

# Course Content

### Endpoint: POST /api/content/upload

**Description:** Upload course content (video, pdf, etc).

**Request Body:** multipart/form-data

- CourseId: string (UUID)  
- Title: string  
- Type: string (video/pdf)  
- File: file

**Responses**

**200: Content uploaded**

```json
{
  "id": "content123",
  "title": "Lesson 1"
}
```

**400:** Invalid data  
**500:** Upload failed

---

### Endpoint: GET /api/content/course/{courseId}

**Description:** Get all content for a course.

**Path Parameters**

- courseId: string (UUID)

**Responses**

**200: Course content list**

```json
[
  {
    "id": "content123",
    "title": "Lesson 1",
    "type": "video"
  }
]
```

**500:** Failed to fetch content

---

### Endpoint: DELETE /api/content/{id}

**Description:** Delete course content.

**Path Parameters**

- id: string (UUID)

**Responses**

**200: Content deleted**

```json
{
  "message": "Content deleted successfully"
}
```

**404:** Content not found  
**500:** Deletion failed


---

# Instructors

### Endpoint: GET /api/instructors/all

**Description:** Get all instructors.

**Responses**

**200: Array of instructors**

```json
[
  {
    "id": "instr123",
    "name": "John Doe",
    "email": "john@example.com"
  }
]
```

**500:** Failed to fetch instructors

---

### Endpoint: POST /api/instructors/create

**Description:** Create a new instructor.

**Request Body:** application/json

```json
{
  "name": "Jane Smith",
  "email": "jane@example.com",
  "bio": "Expert in Physics"
}
```

**Responses**

**200: Instructor created**

```json
{
  "id": "instr124",
  "message": "Instructor created successfully"
}
```

**400:** Invalid data  
**500:** Creation failed

---

### Endpoint: PUT /api/instructors/update/{id}

**Description:** Update instructor details.

**Path Parameters**

- id: string (UUID)

**Request Body:** application/json

```json
{
  "name": "Jane Doe",
  "bio": "Updated bio"
}
```

**Responses**

**200: Updated successfully**

```json
{
  "message": "Instructor updated successfully"
}
```

**404:** Instructor not found  
**500:** Update failed

---

### Endpoint: DELETE /api/instructors/delete/{id}

**Description:** Delete an instructor.

**Path Parameters**

- id: string (UUID)

**Responses**

**200: Deleted successfully**

```json
{
  "message": "Instructor deleted successfully"
}
```

**404:** Instructor not found  
**500:** Deletion failed

---

# Notifications

### Endpoint: GET /api/notifications

**Description:** Get notifications for logged-in user.

**Responses**

**200: Array of notifications**

```json
[
  {
    "id": "notif123",
    "title": "New course available",
    "read": false
  }
]
```

**500:** Failed to fetch notifications

---

### Endpoint: POST /api/notifications/send

**Description:** Send notification to users.

**Request Body:** application/json

```json
{
  "title": "System Update",
  "message": "New features released",
  "userIds": ["user123", "user456"]
}
```

**Responses**

**200: Notification sent**

```json
{
  "message": "Notification sent successfully"
}
```

**400:** Invalid data  
**500:** Sending failed

---

# Scheduling / Classes

### Endpoint: POST /api/schedule/create

**Description:** Create a class schedule.

**Request Body:** application/json

```json
{
  "courseId": "course123",
  "instructorId": "instr123",
  "startTime": "2026-03-15T10:00:00Z",
  "endTime": "2026-03-15T12:00:00Z"
}
```

**Responses**

**200: Schedule created**

```json
{
  "id": "sched123",
  "message": "Schedule created successfully"
}
```

**400:** Invalid data  
**500:** Creation failed

---

### Endpoint: GET /api/schedule/course/{courseId}

**Description:** Get schedule for a course.

**Path Parameters**

- courseId: string (UUID)

**Responses**

**200: Schedule list**

```json
[
  {
    "id": "sched123",
    "startTime": "2026-03-15T10:00:00Z",
    "endTime": "2026-03-15T12:00:00Z",
    "instructorId": "instr123"
  }
]
```

**500:** Failed to fetch schedule

---

### Endpoint: PUT /api/schedule/update/{id}

**Description:** Update a schedule.

**Path Parameters**

- id: string (UUID)

**Request Body:** application/json

```json
{
  "startTime": "2026-03-15T11:00:00Z",
  "endTime": "2026-03-15T13:00:00Z"
}
```

**Responses**

**200: Updated successfully**

```json
{
  "message": "Schedule updated successfully"
}
```

**404:** Schedule not found  
**500:** Update failed

---

### Endpoint: DELETE /api/schedule/delete/{id}

**Description:** Delete a schedule.

**Path Parameters**

- id: string (UUID)

**Responses**

**200: Deleted successfully**

```json
{
  "message": "Schedule deleted successfully"
}
```

**404:** Schedule not found  
**500:** Deletion failed

---

