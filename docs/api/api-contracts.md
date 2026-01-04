# CardSense – API Contracts (v1)

Base URL:
https://api.cardsense.app/v1

Authentication:

Authorization: Bearer <access_token>

All responses follow a **consistent envelope**.

---

## 1. Standard API Response Format

### Success

```json
{
  "success": true,
  "data": {},
  "message": "Optional message",
  "timestamp": "2026-01-04T12:30:45Z",
  "requestId": "uuid"
}
```

### Error

```json
{
  "success": false,
  "message": "Human readable error",
  "errors": [
    {
      "field": "email",
      "reason": "Email already exists"
    }
  ],
  "timestamp": "2026-01-04T12:30:45Z",
  "requestId": "uuid"
}
```

---

## 2. Authentication APIs

### 2.1 Register User

POST /auth/register

```
{
  "fullName": "Rahul Sharma",
  "email": "rahul@gmail.com",
  "phone": "9876543210",
  "password": "StrongPassword@123"
}
```

Response

```
{
  "userId": "uuid",
  "emailVerificationRequired": true
}
```

### 2.2 Login (Email + Password)

POST /auth/login

```
{
  "email": "rahul@gmail.com",
  "password": "StrongPassword@123"
}
```

Response

```
{
  "accessToken": "jwt",
  "refreshToken": "jwt",
  "expiresIn": 900
}
```

### 2.3 Login with OTP

POST /auth/login/otp

```
{
  "phone": "9876543210"
}
```

Response:

```
OTP sent successfully
```

### 2.4 Verify OTP

POST /auth/verify-otp

```
{
  "phone": "9876543210",
  "otp": "123456"
}
```

### 2.5 Refresh Token

POST /auth/refresh

```json
{
  "refreshToken": "jwt"
}
```

### 2.6 Logout

POST /auth/logout

---

## 3. User & Preferences APIs

### 3.1 Get Profile

GET /users/me

### 3.2 Update Preferences

PUT /users/preferences

```
{
  "theme": "dark",
  "language": "en",
  "reminderDaysBefore": 3,
  "pushNotifications": true
}
```

---

## 4. Cards APIs

### 4.1 Add Card

POST /cards

```
{
  "cardName": "HDFC Regalia Gold",
  "cardNickname": "Travel Card",
  "bankName": "HDFC",
  "cardNetwork": "VISA",
  "lastFourDigits": "3456",
  "creditLimit": 200000,
  "billingCycleDay": 5,
  "paymentDueDay": 25
}
```

### 4.2 Get All Cards

GET /cards

### 4.3 Get Card Details

GET /cards/{cardId}

### 4.4 Update Card

PUT /cards/{cardId}

### 4.5 Deactivate Card

DELETE /cards/{cardId}

---

## 5. Transactions APIs

### 5.1 Add Transaction

POST /transactions

```
{
  "cardId": "uuid",
  "amount": 2500,
  "categoryId": "uuid",
  "merchantName": "Amazon",
  "transactionDate": "2026-01-03T18:30:00",
  "notes": "Electronics purchase",
  "isRecurring": false
}
```

### 5.2 Get Transactions (Filters)

GET /transactions

Query Params:

```
?cardId=
&categoryId=
&fromDate=
&toDate=
&page=
&limit=
```

### 5.3 Update Transaction

PUT /transactions/{transactionId}

### 5.4 Delete Transaction

DELETE /transactions/{transactionId}

---

## 6. Categories APIs

### 6.1 Get Categories

GET /categories

### 6.2 Create Custom Category

POST /categories

```
{
  "name": "Subscriptions",
  "icon": "subscriptions",
  "color": "#673AB7",
  "parentCategoryId": "uuid"
}
```

---

## 7. Card Benefits APIs

### 7.1 Get Benefits for a Card

GET /benefits/cards/{cardName}

### 7.2 Add Custom Benefit

POST /benefits/custom

```
{
  "cardId": "uuid",
  "benefitCategory": "cashback",
  "description": "5% cashback on dining till June",
  "value": "5%"
}
```

---

## 8. Recommendation API (Core Feature ⭐)

## 8.1 Which Card Should I Use?

POST /recommendations/best-card

```
{
  "category": "Food & Dining",
  "amount": 2000
}
```

Response

```
{
  "recommendedCards": [
    {
      "cardId": "uuid",
      "cardName": "SBI Cashback",
      "estimatedReward": 100,
      "rewardType": "CASHBACK",
      "reason": "5% cashback on dining"
    }
  ]
}
```

⚠️ This API is data-driven, powered by reward rules, not hardcoded logic.

---

## 9. Reminders APIs

### 9.1 Get Upcoming Reminders

GET /reminders/upcoming

### 9.2 Mark Bill as Paid

POST /reminders/{reminderId}/mark-paid

---

## 10. Notifications APIs

### 10.1 Get Notifications

GET /notifications

### 10.2 Mark Notification Read

POST /notifications/{id}/read

---

## 11. Non-Functional Standards

Rate Limiting

100 requests / minute / user

OTP APIs: 5 requests / hour

Versioning

All APIs under /v1

Breaking changes go to /v2

Pagination

```
{
  "success": true,
  "data": {
    "items": [],
    "page": 1,
    "limit": 20,
    "total": 145
  }
}

```

---

## 12. Summary

The CardSense API:

- Is RESTful and consistent
- Cleanly separates concerns
- Supports future AI recommendations
- Is easy to document via Swagger
- Can scale from MVP to production

```

---
```
