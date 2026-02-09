# NIVISION AI Call Analysis API

API Documentation for NIVISION - Hebrew call transcription and AI-powered analysis platform.

## 🔗 Base URL
```
https://apicore.nivision.co.il
```

## 🔐 Authentication

All API requests require authentication using an API Token in the request headers.

### Header Authentication
```http
API_TOKEN: your_api_token_here
```

### Alternative Header
```http
x-api-key: your_api_token_here
```

### Query Parameter (optional)
```
?API_TOKEN=your_api_token_here
```

> **Note:** You can obtain your API Token from your NIVISION account settings at [nivision.co.il](https://nivision.co.il)

---

## 📡 Endpoints

### Test Connection

Verify your API token and connection status.

**Endpoint:** `GET /v3/test`

**Headers:**
```http
API_TOKEN: your_api_token_here
```

**Success Response:**
```json
{
  "success": true,
  "message": "Connection successful",
  "token_valid": true
}
```

**Error Response:**
```json
{
  "success": false,
  "error": "Invalid API_TOKEN"
}
```

---

### Send Call for Analysis

Submit a call recording for AI-powered transcription and analysis.

**Endpoint:** `POST /v3/sendcall`

**Headers:**
```http
Content-Type: application/json
API_TOKEN: your_api_token_here
```

**Request Body:**
```json
{
  "callid": "call_12345",
  "line_phone_number": "03-1234567",
  "phonenumber": "054-9876543",
  "recordurl": "https://example.com/recording.mp3",
  "call_type": "INBOUND",
  "duration": 180,
  "rep_name": "דני כהן",
  "camp": "Google Ads",
  "createon": "2026-02-09T10:30:00Z"
}
```

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `callid` | string | ✅ Yes | Unique identifier for the call |
| `line_phone_number` | string | ✅ Yes | Phone number of the line/extension |
| `phonenumber` | string | ✅ Yes | Customer's phone number |
| `recordurl` | string (URL) | ✅ Yes | URL to the call recording file (mp3/wav) |
| `call_type` | string | ✅ Yes | Either "INBOUND" or "OUTBOUND" |
| `duration` | number | ⚪ No | Call duration in seconds |
| `rep_name` | string | ⚪ No | Name of the sales agent/representative |
| `camp` | string | ⚪ No | Campaign or lead source |
| `createon` | string | ⚪ No | Call date/time (ISO 8601 format) |

**Success Response:**
```json
{
  "success": true,
  "call_id": "call_12345",
  "status": "processing",
  "message": "Call received and queued for analysis"
}
```

**Error Response:**
```json
{
  "error": "Invalid API_TOKEN or route"
}
```

---

### Webhook: Register for New Calls

Register a webhook to receive notifications when new calls are received.

**Endpoint:** `POST /v3/webhooks/calls`

**Headers:**
```http
Content-Type: application/json
API_TOKEN: your_api_token_here
```

**Request Body:**
```json
{
  "hookUrl": "https://hook.make.com/your-webhook-url",
  "events": ["call.received"]
}
```

**Success Response:**
```json
{
  "success": true,
  "webhook_id": "wh_abc123"
}
```

**Webhook Payload** (sent to your hookUrl):
```json
{
  "callid": "call_12345",
  "line_phone_number": "03-1234567",
  "phonenumber": "054-9876543",
  "recordurl": "https://example.com/recording.mp3",
  "call_type": "INBOUND",
  "duration": 180,
  "rep_name": "דני כהן",
  "camp": "Google Ads",
  "createon": "2026-02-09T10:30:00Z"
}
```

---

### Webhook: Unregister New Calls

Remove a registered webhook.

**Endpoint:** `DELETE /v3/webhooks/calls/{webhook_id}`

**Headers:**
```http
API_TOKEN: your_api_token_here
```

**Success Response:**
```json
{
  "success": true,
  "message": "Webhook deleted"
}
```

---

### Webhook: Register for Reports

Register a webhook to receive notifications when analysis reports are ready.

**Endpoint:** `POST /v3/webhooks/reports`

**Headers:**
```http
Content-Type: application/json
API_TOKEN: your_api_token_here
```

**Request Body:**
```json
{
  "hookUrl": "https://hook.make.com/your-webhook-url",
  "events": ["report.ready"],
  "filter": {
    "report_type": "daily",
    "custom_report_name": ""
  }
}
```

**Filter Parameters:**

| Parameter | Type | Options | Description |
|-----------|------|---------|-------------|
| `report_type` | string | daily, weekly, monthly, custom | Type of report |
| `custom_report_name` | string | - | Required when report_type is "custom" |

**Success Response:**
```json
{
  "success": true,
  "webhook_id": "wh_xyz789"
}
```

**Webhook Payload** (sent to your hookUrl):
```json
{
  "report_id": "rpt_67890",
  "report_type": "daily",
  "report_name": "Daily Analysis Report - Feb 9, 2026",
  "report_url": "https://nivision.co.il/reports/rpt_67890",
  "generated_at": "2026-02-09T23:59:00Z",
  "period_start": "2026-02-09T00:00:00Z",
  "period_end": "2026-02-09T23:59:59Z",
  "total_calls": 145
}
```

---

### Webhook: Unregister Reports

Remove a registered webhook for reports.

**Endpoint:** `DELETE /v3/webhooks/reports/{webhook_id}`

**Headers:**
```http
API_TOKEN: your_api_token_here
```

**Success Response:**
```json
{
  "success": true,
  "message": "Webhook deleted"
}
```

---

## ❌ Error Handling

### Error Status Codes

| Status Code | Description |
|-------------|-------------|
| 401 | Missing API_TOKEN |
| 403 | Invalid API_TOKEN or route |
| 404 | Endpoint not found |
| 500 | Internal server error |

### Error Response Format
```json
{
  "error": "Error description here"
}
```

---

## 🔗 Integration Examples

### Make.com (Integromat)

NIVISION is available as a custom app in Make.com:
1. Search for "NIVISION AI Analysis" in Make
2. Add your API Token
3. Use the available modules:
   - **Send Call to NIVISION** (Action)
   - **Watch New Calls** (Instant Trigger)
   - **Watch New Reports** (Instant Trigger)

---

## 💡 Use Cases

- **Automatic call transcription** - Convert Hebrew calls to text
- **Quality assurance** - Monitor agent performance
- **Compliance monitoring** - Ensure regulatory compliance
- **Sales analysis** - Track successful sales patterns
- **Customer sentiment** - Analyze customer satisfaction

---

## 📞 Support

- **Email:** support@nivision.co.il
- **Website:** [nivision.co.il](https://nivision.co.il)
- **Company:** NIVISION DATA LTD

---

## 📜 Version History

### v3 (Current)
- Hebrew call transcription
- AI-powered analysis
- Webhook support for real-time notifications
- Daily, weekly, monthly, and custom reports

---

**Last Updated:** February 2026
```

---

## 💾 שלב 3: שמור את השינויים

### לחץ על **"Commit changes"** (הכפתור הירוק)

### במסך שנפתח:
- **Commit message:** `Add API documentation`
- לחץ **"Commit changes"**

---

## 🌐 שלב 4: קבל את הקישור לדוקומנטציה

### הקישור שלך יהיה:
```
https://github.com/YOUR_USERNAME/nivision-api-docs
```

**תחליף `YOUR_USERNAME` בשם המשתמש שלך ב-GitHub**

---

## ✨ בונוס: הפעל GitHub Pages (אופציונלי)

אם רוצה URL יפה יותר:

### 1️⃣ Settings → Pages
### 2️⃣ Source: Deploy from a branch
### 3️⃣ Branch: main → / (root) → Save

**אז תקבל:**
```
https://YOUR_USERNAME.github.io/nivision-api-docs/
```

---

## ✅ סיימת! עכשיו:

**העתק את הקישור:**
```
https://github.com/YOUR_USERNAME/nivision-api-docs
