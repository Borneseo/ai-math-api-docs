# 📡 API 명세서 – 초등 수학 AI 학습도우미 (MVP)

## ✅ 공통 정보
- **Base URL**: `https://api.ai-math-tutor.com`
- **Content-Type**: `application/json`
- **인증 방식**: Firebase ID Token 또는 JWT (`Authorization: Bearer <token>`)

---

## 🧒 학습자 기능 API

### 📥 문제 업로드
**POST** `/api/problem/upload`
- Request: `multipart/form-data` (file) 또는 `{ text: string }`
- Response: `{ uploadId: string, previewUrl?: string }`

---

### 🔍 OCR + 문제 인식
**POST** `/api/problem/ocr`
- Body: `{ uploadId: string }`
- Response: `{ questions: [ { id, text, imageUrl } ] }`

---

### ✅ 채점 요청
**POST** `/api/answer/check`
- Body: `{ answers: [ { questionId, userAnswer } ] }`
- Response: `{ results: [ { questionId, isCorrect, correctAnswer } ] }`

---

### 💬 해설 요청
**POST** `/api/explanation`
- Body: `{ questionText, userAnswer, correctAnswer, gradeLevel }`
- Response: `{ strategy, solution, childExplanation }`

---

### 🎖 스티커 보상 저장
**POST** `/api/reward/sticker`
- Body: `{ userId, reason: string, points: number }`
- Response: `{ success: true, total: number }`

---

## 👩 보호자 기능 API

### 📊 학습 리포트 조회
**GET** `/api/report/summary?userId=123`
- Response:
```json
{
  "today": { "total": 8, "correct": 6, "timeSpent": 540 },
  "weeklyTrend": [ { "date": "2025-04-22", "correct": 6 } ],
  "mistakeTypes": [ "덧셈자리올림", "세로식" ]
}
```

---

### 📄 개별 문제 해설 열람
**GET** `/api/explanation/detail?questionId=abc123`
- Response:
```json
{
  "strategy": "십의 자리 먼저 계산",
  "solution": "40+20=60, 8+7=15, 60+15=75",
  "childExplanation": "큰 수 먼저 더하고 작은 수 더하면 75야!",
  "parentNote": "자리올림 개념을 강화해주세요."
}
```

---

### 📥 자녀 학습 등록/연동
**POST** `/api/parent/link-child`
- Body: `{ parentId, childCode }`
- Response: `{ success: true, childName }`

---

## 🔒 인증 API

### 🔐 Firebase Token 확인
**POST** `/api/auth/verify`
- Body: `{ token: string }`
- Response: `{ uid: string, userType: 'parent' | 'child' }`

---

## 📁 파일 업로드 (S3 Presigned URL 방식)

### **GET** `/api/upload-url`
- Query: `type=image/pdf`, `filename=string`
- Response: `{ url, fileKey }`

---

## 📘 참고
- 모든 POST 요청 성공 시: `200 OK`
- 실패 시: `{ error: 'reason' }`
- 추후: `채점+해설 통합 API` 가능성 있음 (LLM latency 고려) 