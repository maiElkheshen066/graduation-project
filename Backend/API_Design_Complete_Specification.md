# API Design: Complete REST API Specification

## Overview
This document provides a comprehensive specification for all REST API endpoints in the ChildGuard Real-Time Child Monitoring and Safety System.

---

## **1. API Overview**

### **1.1 Base Configuration**
```
Base URL: https://api.childguard.com/v1
Authentication: Bearer Token (JWT)
Content-Type: application/json
API Version: v1
Rate Limiting: 1000 requests per hour per user
CORS: Enabled for mobile app domains
```

### **1.2 Authentication Scheme**
```
Authorization: Bearer <jwt_token>
X-API-Key: <api_key> (for IoT devices)
X-Device-ID: <device_id> (for IoT devices)
```

### **1.3 Response Format**
```json
{
  "success": true,
  "message": "Operation completed successfully",
  "data": { ... },
  "timestamp": "2024-01-01T10:30:00Z",
  "requestId": "req_123456789"
}
```

---

## **2. Authentication Endpoints**

### **2.1 User Registration**
```http
POST /auth/register
Content-Type: application/json

Request Body:
{
  "email": "parent@example.com",
  "password": "StrongPass123!",
  "firstName": "John",
  "lastName": "Doe",
  "phoneNumber": "+1234567890",
  "dateOfBirth": "1985-05-15"
}

Response (201 Created):
{
  "success": true,
  "message": "User registered successfully",
  "data": {
    "userId": "550e8400-e29b-41d4-a716-446655440000",
    "email": "parent@example.com",
    "verificationRequired": true,
    "verificationToken": "verification_token_here"
  }
}

Error Responses:
- 400: Validation error (invalid email, weak password)
- 409: Email already exists
```

### **2.2 User Login**
```http
POST /auth/login
Content-Type: application/json

Request Body:
{
  "email": "parent@example.com",
  "password": "StrongPass123!"
}

Response (200 OK):
{
  "success": true,
  "message": "Login successful",
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "refresh_token_here",
    "expiresIn": 86400,
    "tokenType": "Bearer",
    "user": {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "email": "parent@example.com",
      "firstName": "John",
      "lastName": "Doe",
      "isVerified": true
    }
  }
}

Error Responses:
- 401: Invalid credentials
- 403: Account not verified
- 429: Too many login attempts
```

### **2.3 Refresh Token**
```http
POST /auth/refresh
Authorization: Bearer <refresh_token>

Response (200 OK):
{
  "success": true,
  "data": {
    "accessToken": "new_jwt_token",
    "expiresIn": 86400,
    "tokenType": "Bearer"
  }
}

Error Responses:
- 401: Invalid or expired refresh token
```

### **2.4 Logout**
```http
POST /auth/logout
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "message": "Logged out successfully"
}
```

### **2.5 Email Verification**
```http
POST /auth/verify-email
Content-Type: application/json

Request Body:
{
  "token": "verification_token_here"
}

Response (200 OK):
{
  "success": true,
  "message": "Email verified successfully"
}

Error Responses:
- 400: Invalid or expired token
```

### **2.6 Forgot Password**
```http
POST /auth/forgot-password
Content-Type: application/json

Request Body:
{
  "email": "parent@example.com"
}

Response (200 OK):
{
  "success": true,
  "message": "Password reset email sent"
}
```

### **2.7 Reset Password**
```http
POST /auth/reset-password
Content-Type: application/json

Request Body:
{
  "token": "reset_token_here",
  "newPassword": "NewStrongPass123!"
}

Response (200 OK):
{
  "success": true,
  "message": "Password reset successfully"
}
```

---

## **3. User Management Endpoints**

### **3.1 Get User Profile**
```http
GET /users/profile
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "parent@example.com",
    "firstName": "John",
    "lastName": "Doe",
    "phoneNumber": "+1234567890",
    "dateOfBirth": "1985-05-15",
    "profilePictureUrl": "https://storage.childguard.com/profiles/user_123.jpg",
    "isVerified": true,
    "createdAt": "2024-01-01T00:00:00Z",
    "updatedAt": "2024-01-01T00:00:00Z",
    "lastLoginAt": "2024-01-01T10:30:00Z"
  }
}
```

### **3.2 Update User Profile**
```http
PUT /users/profile
Authorization: Bearer <access_token>
Content-Type: application/json

Request Body:
{
  "firstName": "John",
  "lastName": "Doe",
  "phoneNumber": "+1234567890",
  "dateOfBirth": "1985-05-15"
}

Response (200 OK):
{
  "success": true,
  "message": "Profile updated successfully",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "firstName": "John",
    "lastName": "Doe",
    "phoneNumber": "+1234567890",
    "updatedAt": "2024-01-01T10:35:00Z"
  }
}
```

### **3.3 Change Password**
```http
PUT /users/change-password
Authorization: Bearer <access_token>
Content-Type: application/json

Request Body:
{
  "currentPassword": "OldPass123!",
  "newPassword": "NewStrongPass123!"
}

Response (200 OK):
{
  "success": true,
  "message": "Password changed successfully"
}
```

### **3.4 Upload Profile Picture**
```http
POST /users/profile-picture
Authorization: Bearer <access_token>
Content-Type: multipart/form-data

Request Body:
- file: image file (JPEG, PNG, max 5MB)

Response (200 OK):
{
  "success": true,
  "message": "Profile picture uploaded successfully",
  "data": {
    "profilePictureUrl": "https://storage.childguard.com/profiles/user_123.jpg"
  }
}
```

---

## **4. Child Management Endpoints**

### **4.1 Get Children List**
```http
GET /children
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "data": [
    {
      "id": "660e8400-e29b-41d4-a716-446655440000",
      "name": "Emma",
      "dateOfBirth": "2015-03-15",
      "gender": "female",
      "profilePictureUrl": "https://storage.childguard.com/profiles/child_123.jpg",
      "emergencyContactName": "John Doe",
      "emergencyContactPhone": "+1234567890",
      "medicalNotes": "Allergic to peanuts",
      "isActive": true,
      "createdAt": "2024-01-01T00:00:00Z",
      "device": {
        "id": "770e8400-e29b-41d4-a716-446655440000",
        "deviceId": "CHILD_001",
        "deviceName": "Emma's Device",
        "isOnline": true,
        "batteryLevel": 85,
        "lastSeenAt": "2024-01-01T10:30:00Z",
        "lastLocation": {
          "latitude": 30.012345,
          "longitude": 31.123456,
          "accuracy": 5.2,
          "timestamp": "2024-01-01T10:30:00Z"
        }
      }
    }
  ]
}
```

### **4.2 Get Child Details**
```http
GET /children/{childId}
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "data": {
    "id": "660e8400-e29b-41d4-a716-446655440000",
    "name": "Emma",
    "dateOfBirth": "2015-03-15",
    "gender": "female",
    "profilePictureUrl": "https://storage.childguard.com/profiles/child_123.jpg",
    "emergencyContactName": "John Doe",
    "emergencyContactPhone": "+1234567890",
    "medicalNotes": "Allergic to peanuts",
    "isActive": true,
    "createdAt": "2024-01-01T00:00:00Z",
    "updatedAt": "2024-01-01T00:00:00Z",
    "device": {
      "id": "770e8400-e29b-41d4-a716-446655440000",
      "deviceId": "CHILD_001",
      "deviceName": "Emma's Device",
      "isOnline": true,
      "batteryLevel": 85,
      "lastSeenAt": "2024-01-01T10:30:00Z",
      "lastLocation": {
        "latitude": 30.012345,
        "longitude": 31.123456,
        "accuracy": 5.2,
        "timestamp": "2024-01-01T10:30:00Z"
      }
    },
    "geofences": [
      {
        "id": "880e8400-e29b-41d4-a716-446655440000",
        "name": "Home",
        "description": "Home area",
        "centerLatitude": 30.012345,
        "centerLongitude": 31.123456,
        "radiusMeters": 100.0,
        "isActive": true
      }
    ]
  }
}
```

### **4.3 Create Child**
```http
POST /children
Authorization: Bearer <access_token>
Content-Type: application/json

Request Body:
{
  "name": "Emma",
  "dateOfBirth": "2015-03-15",
  "gender": "female",
  "emergencyContactName": "John Doe",
  "emergencyContactPhone": "+1234567890",
  "medicalNotes": "Allergic to peanuts"
}

Response (201 Created):
{
  "success": true,
  "message": "Child created successfully",
  "data": {
    "id": "660e8400-e29b-41d4-a716-446655440000",
    "name": "Emma",
    "dateOfBirth": "2015-03-15",
    "gender": "female",
    "createdAt": "2024-01-01T00:00:00Z"
  }
}
```

### **4.4 Update Child**
```http
PUT /children/{childId}
Authorization: Bearer <access_token>
Content-Type: application/json

Request Body:
{
  "name": "Emma Smith",
  "emergencyContactPhone": "+1234567891",
  "medicalNotes": "Allergic to peanuts and tree nuts"
}

Response (200 OK):
{
  "success": true,
  "message": "Child updated successfully",
  "data": {
    "id": "660e8400-e29b-41d4-a716-446655440000",
    "name": "Emma Smith",
    "emergencyContactPhone": "+1234567891",
    "updatedAt": "2024-01-01T10:35:00Z"
  }
}
```

### **4.5 Delete Child**
```http
DELETE /children/{childId}
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "message": "Child deleted successfully"
}
```

### **4.6 Upload Child Picture**
```http
POST /children/{childId}/picture
Authorization: Bearer <access_token>
Content-Type: multipart/form-data

Request Body:
- file: image file (JPEG, PNG, max 5MB)

Response (200 OK):
{
  "success": true,
  "message": "Child picture uploaded successfully",
  "data": {
    "profilePictureUrl": "https://storage.childguard.com/profiles/child_123.jpg"
  }
}
```

---

## **5. Device Management Endpoints**

### **5.1 Get Devices**
```http
GET /devices
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "data": [
    {
      "id": "770e8400-e29b-41d4-a716-446655440000",
      "deviceId": "CHILD_001",
      "deviceName": "Emma's Device",
      "childId": "660e8400-e29b-41d4-a716-446655440000",
      "childName": "Emma",
      "deviceType": "ESP32",
      "firmwareVersion": "1.2.0",
      "hardwareVersion": "1.0",
      "isOnline": true,
      "batteryLevel": 85,
      "lastSeenAt": "2024-01-01T10:30:00Z",
      "lastLocation": {
        "latitude": 30.012345,
        "longitude": 31.123456,
        "accuracy": 5.2,
        "timestamp": "2024-01-01T10:30:00Z"
      },
      "wifiSignalStrength": -45,
      "isPaired": true,
      "pairingDate": "2024-01-01T00:00:00Z",
      "createdAt": "2024-01-01T00:00:00Z"
    }
  ]
}
```

### **5.2 Get Device Details**
```http
GET /devices/{deviceId}
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "data": {
    "id": "770e8400-e29b-41d4-a716-446655440000",
    "deviceId": "CHILD_001",
    "deviceName": "Emma's Device",
    "childId": "660e8400-e29b-41d4-a716-446655440000",
    "childName": "Emma",
    "deviceType": "ESP32",
    "firmwareVersion": "1.2.0",
    "hardwareVersion": "1.0",
    "isOnline": true,
    "batteryLevel": 85,
    "lastSeenAt": "2024-01-01T10:30:00Z",
    "lastLocation": {
      "latitude": 30.012345,
      "longitude": 31.123456,
      "accuracy": 5.2,
      "timestamp": "2024-01-01T10:30:00Z"
    },
    "wifiSignalStrength": -45,
    "isPaired": true,
    "pairingDate": "2024-01-01T00:00:00Z",
    "createdAt": "2024-01-01T00:00:00Z",
    "updatedAt": "2024-01-01T10:30:00Z"
  }
}
```

### **5.3 Pair Device**
```http
POST /devices/pair
Authorization: Bearer <access_token>
Content-Type: application/json

Request Body:
{
  "childId": "660e8400-e29b-41d4-a716-446655440000",
  "deviceId": "CHILD_001",
  "pairingCode": "ABC123",
  "deviceName": "Emma's Device"
}

Response (200 OK):
{
  "success": true,
  "message": "Device paired successfully",
  "data": {
    "deviceId": "CHILD_001",
    "childId": "660e8400-e29b-41d4-a716-446655440000",
    "pairingDate": "2024-01-01T00:00:00Z"
  }
}
```

### **5.4 Unpair Device**
```http
DELETE /devices/{deviceId}/pair
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "message": "Device unpaired successfully"
}
```

### **5.5 Update Device Settings**
```http
PUT /devices/{deviceId}/settings
Authorization: Bearer <access_token>
Content-Type: application/json

Request Body:
{
  "deviceName": "Emma's New Device",
  "locationUpdateInterval": 30,
  "batteryAlertThreshold": 20,
  "sosEnabled": true,
  "soundDetectionEnabled": true
}

Response (200 OK):
{
  "success": true,
  "message": "Device settings updated successfully"
}
```

### **5.6 Get Device Status**
```http
GET /devices/{deviceId}/status
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "data": {
    "deviceId": "CHILD_001",
    "isOnline": true,
    "batteryLevel": 85,
    "wifiSignalStrength": -45,
    "lastSeenAt": "2024-01-01T10:30:00Z",
    "uptime": 86400,
    "firmwareVersion": "1.2.0",
    "locationUpdateInterval": 30,
    "sosEnabled": true,
    "soundDetectionEnabled": true
  }
}
```

---

## **6. Location Tracking Endpoints**

### **6.1 Get Current Location**
```http
GET /locations/{childId}/current
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "data": {
    "childId": "660e8400-e29b-41d4-a716-446655440000",
    "childName": "Emma",
    "location": {
      "latitude": 30.012345,
      "longitude": 31.123456,
      "accuracy": 5.2,
      "altitude": 25.5,
      "speed": 2.5,
      "heading": 180.0,
      "satellites": 8,
      "timestamp": "2024-01-01T10:30:00Z"
    },
    "device": {
      "batteryLevel": 85,
      "wifiSignal": -45,
      "isOnline": true
    },
    "geofences": [
      {
        "id": "880e8400-e29b-41d4-a716-446655440000",
        "name": "Home",
        "isInside": true,
        "distance": 25.5
      }
    ]
  }
}
```

### **6.2 Get Location History**
```http
GET /locations/{childId}/history
Authorization: Bearer <access_token>
Query Parameters:
- startDate: 2024-01-01T00:00:00Z
- endDate: 2024-01-01T23:59:59Z
- limit: 100 (default: 50, max: 500)
- offset: 0
- includeGeofences: true|false (default: false)

Response (200 OK):
{
  "success": true,
  "data": {
    "childId": "660e8400-e29b-41d4-a716-446655440000",
    "childName": "Emma",
    "locations": [
      {
        "latitude": 30.012345,
        "longitude": 31.123456,
        "accuracy": 5.2,
        "speed": 2.5,
        "heading": 180.0,
        "timestamp": "2024-01-01T10:30:00Z",
        "geofences": [
          {
            "id": "880e8400-e29b-41d4-a716-446655440000",
            "name": "Home",
            "isInside": true
          }
        ]
      }
    ],
    "pagination": {
      "total": 1000,
      "limit": 100,
      "offset": 0,
      "hasMore": true
    }
  }
}
```

### **6.3 Get Location Statistics**
```http
GET /locations/{childId}/statistics
Authorization: Bearer <access_token>
Query Parameters:
- startDate: 2024-01-01T00:00:00Z
- endDate: 2024-01-01T23:59:59Z

Response (200 OK):
{
  "success": true,
  "data": {
    "childId": "660e8400-e29b-41d4-a716-446655440000",
    "totalDistance": 5.2,
    "averageSpeed": 2.1,
    "maxSpeed": 15.5,
    "totalTime": 86400,
    "mostVisitedPlaces": [
      {
        "latitude": 30.012345,
        "longitude": 31.123456,
        "name": "Home",
        "visitCount": 15,
        "totalTime": 43200
      }
    ]
  }
}
```

---

## **7. Geofence Management Endpoints**

### **7.1 Get Geofences**
```http
GET /geofences
Authorization: Bearer <access_token>
Query Parameters:
- childId: uuid (optional)
- isActive: true|false (optional)

Response (200 OK):
{
  "success": true,
  "data": [
    {
      "id": "880e8400-e29b-41d4-a716-446655440000",
      "childId": "660e8400-e29b-41d4-a716-446655440000",
      "childName": "Emma",
      "name": "Home",
      "description": "Home area",
      "center": {
        "latitude": 30.012345,
        "longitude": 31.123456
      },
      "radiusMeters": 100.0,
      "isActive": true,
      "alertOnEntry": false,
      "alertOnExit": true,
      "createdAt": "2024-01-01T00:00:00Z",
      "updatedAt": "2024-01-01T00:00:00Z"
    }
  ]
}
```

### **7.2 Get Geofence Details**
```http
GET /geofences/{geofenceId}
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "data": {
    "id": "880e8400-e29b-41d4-a716-446655440000",
    "childId": "660e8400-e29b-41d4-a716-446655440000",
    "childName": "Emma",
    "name": "Home",
    "description": "Home area",
    "centerLatitude": 30.012345,
    "centerLongitude": 31.123456,
    "radiusMeters": 100.0,
    "isActive": true,
    "alertOnEntry": false,
    "alertOnExit": true,
    "createdAt": "2024-01-01T00:00:00Z",
    "updatedAt": "2024-01-01T00:00:00Z",
    "violations": [
      {
        "id": "990e8400-e29b-41d4-a716-446655440000",
        "violationType": "exit",
        "timestamp": "2024-01-01T10:30:00Z",
        "location": {
          "latitude": 30.012345,
          "longitude": 31.123456
        }
      }
    ]
  }
}
```

### **7.3 Create Geofence**
```http
POST /geofences
Authorization: Bearer <access_token>
Content-Type: application/json

Request Body:
{
  "childId": "660e8400-e29b-41d4-a716-446655440000",
  "name": "School",
  "description": "School area",
  "centerLatitude": 30.012345,
  "centerLongitude": 31.123456,
  "radiusMeters": 200.0,
  "alertOnEntry": true,
  "alertOnExit": true
}

Response (201 Created):
{
  "success": true,
  "message": "Geofence created successfully",
  "data": {
    "id": "880e8400-e29b-41d4-a716-446655440000",
    "name": "School",
    "centerLatitude": 30.012345,
    "centerLongitude": 31.123456,
    "radiusMeters": 200.0,
    "createdAt": "2024-01-01T00:00:00Z"
  }
}
```

### **7.4 Update Geofence**
```http
PUT /geofences/{geofenceId}
Authorization: Bearer <access_token>
Content-Type: application/json

Request Body:
{
  "name": "School Updated",
  "description": "Updated school area",
  "radiusMeters": 250.0,
  "alertOnEntry": false
}

Response (200 OK):
{
  "success": true,
  "message": "Geofence updated successfully",
  "data": {
    "id": "880e8400-e29b-41d4-a716-446655440000",
    "name": "School Updated",
    "radiusMeters": 250.0,
    "updatedAt": "2024-01-01T10:35:00Z"
  }
}
```

### **7.5 Delete Geofence**
```http
DELETE /geofences/{geofenceId}
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "message": "Geofence deleted successfully"
}
```

### **7.6 Get Geofence Violations**
```http
GET /geofences/{geofenceId}/violations
Authorization: Bearer <access_token>
Query Parameters:
- startDate: 2024-01-01T00:00:00Z
- endDate: 2024-01-01T23:59:59Z
- violationType: entry|exit (optional)
- limit: 50 (default: 50, max: 200)
- offset: 0

Response (200 OK):
{
  "success": true,
  "data": [
    {
      "id": "990e8400-e29b-41d4-a716-446655440000",
      "geofenceId": "880e8400-e29b-41d4-a716-446655440000",
      "geofenceName": "Home",
      "violationType": "exit",
      "location": {
        "latitude": 30.012345,
        "longitude": 31.123456,
        "accuracy": 5.2
      },
      "timestamp": "2024-01-01T10:30:00Z",
      "isAcknowledged": false,
      "acknowledgedBy": null,
      "acknowledgedAt": null
    }
  ],
  "pagination": {
    "total": 150,
    "limit": 50,
    "offset": 0,
    "hasMore": true
  }
}
```

---

## **8. Alert Management Endpoints**

### **8.1 Get Alerts**
```http
GET /alerts
Authorization: Bearer <access_token>
Query Parameters:
- childId: uuid (optional)
- alertType: sos|geofence_violation|sound_detection|proximity_alert|battery_low|device_offline (optional)
- severity: low|medium|high|critical (optional)
- isRead: true|false (optional)
- isAcknowledged: true|false (optional)
- startDate: 2024-01-01T00:00:00Z (optional)
- endDate: 2024-01-01T23:59:59Z (optional)
- limit: 50 (default: 50, max: 200)
- offset: 0

Response (200 OK):
{
  "success": true,
  "data": [
    {
      "id": "aa0e8400-e29b-41d4-a716-446655440000",
      "childId": "660e8400-e29b-41d4-a716-446655440000",
      "childName": "Emma",
      "deviceId": "CHILD_001",
      "alertType": "sos",
      "severity": "critical",
      "title": "SOS Alert",
      "message": "Emergency alert from Emma's device",
      "location": {
        "latitude": 30.012345,
        "longitude": 31.123456,
        "accuracy": 5.2
      },
      "isRead": false,
      "isAcknowledged": false,
      "acknowledgedBy": null,
      "acknowledgedAt": null,
      "pushSent": true,
      "pushSentAt": "2024-01-01T10:30:00Z",
      "createdAt": "2024-01-01T10:30:00Z"
    }
  ],
  "pagination": {
    "total": 150,
    "limit": 50,
    "offset": 0,
    "hasMore": true
  }
}
```

### **8.2 Get Alert Details**
```http
GET /alerts/{alertId}
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "data": {
    "id": "aa0e8400-e29b-41d4-a716-446655440000",
    "childId": "660e8400-e29b-41d4-a716-446655440000",
    "childName": "Emma",
    "deviceId": "CHILD_001",
    "alertType": "sos",
    "severity": "critical",
    "title": "SOS Alert",
    "message": "Emergency alert from Emma's device",
    "location": {
      "latitude": 30.012345,
      "longitude": 31.123456,
      "accuracy": 5.2
    },
    "isRead": false,
    "isAcknowledged": false,
    "acknowledgedBy": null,
    "acknowledgedAt": null,
    "pushSent": true,
    "pushSentAt": "2024-01-01T10:30:00Z",
    "createdAt": "2024-01-01T10:30:00Z",
    "updatedAt": "2024-01-01T10:30:00Z"
  }
}
```

### **8.3 Mark Alert as Read**
```http
PUT /alerts/{alertId}/read
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "message": "Alert marked as read",
  "data": {
    "alertId": "aa0e8400-e29b-41d4-a716-446655440000",
    "readAt": "2024-01-01T10:35:00Z"
  }
}
```

### **8.4 Acknowledge Alert**
```http
PUT /alerts/{alertId}/acknowledge
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "message": "Alert acknowledged successfully",
  "data": {
    "alertId": "aa0e8400-e29b-41d4-a716-446655440000",
    "acknowledgedAt": "2024-01-01T10:35:00Z"
  }
}
```

### **8.5 Get Alert Statistics**
```http
GET /alerts/statistics
Authorization: Bearer <access_token>
Query Parameters:
- startDate: 2024-01-01T00:00:00Z
- endDate: 2024-01-01T23:59:59Z
- childId: uuid (optional)

Response (200 OK):
{
  "success": true,
  "data": {
    "totalAlerts": 150,
    "unreadAlerts": 25,
    "unacknowledgedAlerts": 10,
    "alertsByType": {
      "sos": 5,
      "geofence_violation": 45,
      "sound_detection": 30,
      "proximity_alert": 20,
      "battery_low": 25,
      "device_offline": 25
    },
    "alertsBySeverity": {
      "critical": 10,
      "high": 35,
      "medium": 60,
      "low": 45
    },
    "alertsByChild": [
      {
        "childId": "660e8400-e29b-41d4-a716-446655440000",
        "childName": "Emma",
        "totalAlerts": 75,
        "unreadAlerts": 15
      }
    ]
  }
}
```

---

## **9. SOS Events Endpoints**

### **9.1 Get SOS Events**
```http
GET /sos-events
Authorization: Bearer <access_token>
Query Parameters:
- childId: uuid (optional)
- isResolved: true|false (optional)
- startDate: 2024-01-01T00:00:00Z (optional)
- endDate: 2024-01-01T23:59:59Z (optional)
- limit: 50 (default: 50, max: 200)
- offset: 0

Response (200 OK):
{
  "success": true,
  "data": [
    {
      "id": "bb0e8400-e29b-41d4-a716-446655440000",
      "deviceId": "CHILD_001",
      "childId": "660e8400-e29b-41d4-a716-446655440000",
      "childName": "Emma",
      "location": {
        "latitude": 30.012345,
        "longitude": 31.123456,
        "accuracy": 5.2
      },
      "batteryLevel": 85,
      "timestamp": "2024-01-01T10:30:00Z",
      "isResolved": false,
      "resolvedBy": null,
      "resolvedAt": null,
      "resolutionNotes": null
    }
  ],
  "pagination": {
    "total": 25,
    "limit": 50,
    "offset": 0,
    "hasMore": false
  }
}
```

### **9.2 Resolve SOS Event**
```http
PUT /sos-events/{eventId}/resolve
Authorization: Bearer <access_token>
Content-Type: application/json

Request Body:
{
  "resolutionNotes": "Child was found safe at school"
}

Response (200 OK):
{
  "success": true,
  "message": "SOS event resolved successfully",
  "data": {
    "eventId": "bb0e8400-e29b-41d4-a716-446655440000",
    "resolvedAt": "2024-01-01T10:35:00Z",
    "resolutionNotes": "Child was found safe at school"
  }
}
```

---

## **10. Sound Detection Endpoints**

### **10.1 Get Sound Events**
```http
GET /sound-events
Authorization: Bearer <access_token>
Query Parameters:
- childId: uuid (optional)
- soundType: crying|screaming|silence|unusual_noise|glass_breaking (optional)
- confidenceLevel: min value (0.0-1.0) (optional)
- startDate: 2024-01-01T00:00:00Z (optional)
- endDate: 2024-01-01T23:59:59Z (optional)
- limit: 50 (default: 50, max: 200)
- offset: 0

Response (200 OK):
{
  "success": true,
  "data": [
    {
      "id": "cc0e8400-e29b-41d4-a716-446655440000",
      "deviceId": "CHILD_001",
      "childId": "660e8400-e29b-41d4-a716-446655440000",
      "childName": "Emma",
      "soundType": "crying",
      "confidenceLevel": 0.85,
      "durationSeconds": 30,
      "amplitude": 75.5,
      "frequencyRange": "200-800Hz",
      "location": {
        "latitude": 30.012345,
        "longitude": 31.123456
      },
      "timestamp": "2024-01-01T10:30:00Z",
      "isAlertGenerated": true
    }
  ],
  "pagination": {
    "total": 100,
    "limit": 50,
    "offset": 0,
    "hasMore": true
  }
}
```

---

## **11. System Configuration Endpoints**

### **11.1 Get App Configuration**
```http
GET /config/app
Authorization: Bearer <access_token>

Response (200 OK):
{
  "success": true,
  "data": {
    "locationUpdateInterval": 30,
    "batteryAlertThreshold": 20,
    "sosEnabled": true,
    "soundDetectionEnabled": true,
    "geofenceEnabled": true,
    "pushNotificationsEnabled": true,
    "maxGeofencesPerChild": 10,
    "locationHistoryRetentionDays": 90,
    "alertRetentionDays": 365
  }
}
```

### **11.2 Update App Configuration**
```http
PUT /config/app
Authorization: Bearer <access_token>
Content-Type: application/json

Request Body:
{
  "locationUpdateInterval": 60,
  "batteryAlertThreshold": 15,
  "sosEnabled": true,
  "soundDetectionEnabled": false
}

Response (200 OK):
{
  "success": true,
  "message": "App configuration updated successfully"
}
```

---

## **12. Error Responses**

### **12.1 Standard Error Format**
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      {
        "field": "email",
        "message": "Email format is invalid"
      }
    ]
  },
  "timestamp": "2024-01-01T10:30:00Z",
  "requestId": "req_123456789"
}
```

### **12.2 Common Error Codes**
```
AUTHENTICATION_ERROR: Invalid or expired token
AUTHORIZATION_ERROR: Insufficient permissions
VALIDATION_ERROR: Invalid input data
NOT_FOUND: Resource not found
CONFLICT: Resource conflict
RATE_LIMIT_EXCEEDED: Too many requests
INTERNAL_SERVER_ERROR: Server error
DEVICE_OFFLINE: Device is not online
BATTERY_LOW: Device battery is low
GPS_SIGNAL_LOST: GPS signal is weak
```

### **12.3 HTTP Status Codes**
```
200: OK - Request successful
201: Created - Resource created successfully
400: Bad Request - Invalid input
401: Unauthorized - Authentication required
403: Forbidden - Insufficient permissions
404: Not Found - Resource not found
409: Conflict - Resource conflict
429: Too Many Requests - Rate limit exceeded
500: Internal Server Error - Server error
```

---

## **13. Rate Limiting**

### **13.1 Rate Limits**
```
Authentication Endpoints:
- /auth/login: 5 requests per 15 minutes
- /auth/register: 3 requests per hour
- /auth/forgot-password: 3 requests per hour

General Endpoints:
- GET requests: 1000 per hour
- POST/PUT/DELETE requests: 500 per hour
- Location updates: 100 per minute
- Alert acknowledgments: 50 per minute
```

### **13.2 Rate Limit Headers**
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1640995200
Retry-After: 60
```

---

## **14. WebSocket Endpoints**

### **14.1 Real-time Location Updates**
```
WebSocket URL: wss://api.childguard.com/v1/ws/location
Authentication: Bearer <access_token>

Message Format:
{
  "type": "location_update",
  "data": {
    "childId": "660e8400-e29b-41d4-a716-446655440000",
    "location": {
      "latitude": 30.012345,
      "longitude": 31.123456,
      "accuracy": 5.2,
      "timestamp": "2024-01-01T10:30:00Z"
    }
  }
}
```

### **14.2 Real-time Alerts**
```
WebSocket URL: wss://api.childguard.com/v1/ws/alerts
Authentication: Bearer <access_token>

Message Format:
{
  "type": "alert",
  "data": {
    "alertId": "aa0e8400-e29b-41d4-a716-446655440000",
    "childId": "660e8400-e29b-41d4-a716-446655440000",
    "alertType": "sos",
    "severity": "critical",
    "title": "SOS Alert",
    "timestamp": "2024-01-01T10:30:00Z"
  }
}
```

---

**Document Version**: 1.0  
**Last Updated**: December 2024  
**Project**: ChildGuard - Real-Time Child Monitoring System  
**Status**: API Design Complete
