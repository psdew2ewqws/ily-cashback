# WhatsApp Auto-Trigger System

Complete guide to understanding and working with WhatsApp-triggered automatic cashback transactions.

---

## What is WhatsApp Auto-Trigger?

WhatsApp Auto-Trigger is a **backend-driven system** that allows transactions to be automatically processed in ILY Cash when the backend receives transaction data via WhatsApp.

**How it works:**

```
Customer sends WhatsApp → Backend receives → Backend sends via WebSocket → ILY Cash auto-processes
```

**Important**: ILY Cash does NOT read or parse WhatsApp messages directly. The backend server handles WhatsApp integration and sends structured transaction data to the app via Ably WebSocket.

---

## System Architecture

### Backend Flow

1. **Customer or merchant sends WhatsApp message** to backend system
2. **Backend validates** and extracts transaction data
3. **Backend sends structured message** via Ably WebSocket to specific branch channel
4. **ILY Cash receives message** through active WebSocket connection
5. **AutoFlowService processes** transaction automatically
6. **Result dialog** shows success or error

### Technical Components

**WebSocket Connection:**
- **Provider**: Ably (wss://realtime.ably.io)
- **Channel Pattern**: `channel-{branchId}`
- **Connection**: Established after login, reconnects on branch change
- **Status**: Green indicator on home screen = connected

**Auto-Flow Service:**
- **File**: `lib/services/auto_flow_service.dart`
- **Role**: Handles automatic transaction processing
- **Trigger**: Receives WebSocket messages, navigates to earn page, auto-submits form

**Phone Number Indicator:**
- **Auto-triggered transactions**: Phone starts with `E-` prefix
- **Example**: `E-0791234567` indicates backend-triggered transaction
- **Manual transactions**: Regular format `0791234567`

---

## How to Identify Auto-Triggered Transactions

### Visual Indicators

**1. Phone Number Format**

![Auto Phone](../../images/screenshots/cashback-enter-phone.png)

When you see phone number starting with `E-`:
- Example: `E-0790000001`
- This means: Backend sent this transaction via WhatsApp
- Form will auto-submit (no manual input needed)

**2. System Tray Notification**

When auto-trigger occurs:
- Windows notification appears in system tray
- Message: "New transaction received"
- App window auto-focuses

**3. Automatic Navigation**

- App automatically navigates to Earn Cashback page
- Form is pre-filled with data from backend
- Auto-submit happens within 2-3 seconds

---

## User Experience: What Happens

### Scenario: Backend Sends WhatsApp Transaction

**Step 1: You're Working**
- ILY Cash is running (minimized or visible)
- You're serving other customers or doing other tasks

**Step 2: Notification Appears**
- System tray notification: "New transaction received"
- ILY Cash window auto-focuses (comes to front)

**Step 3: Automatic Processing**
- App navigates to Earn Cashback page
- You see phone number with `E-` prefix
- Form auto-fills and submits (no input needed)
- Processing takes 3-5 seconds

**Step 4: Result Dialog**

**Success:**

![Success](../../images/screenshots/cashback-success.png)

Dialog shows:
- Transaction confirmation
- Bill number processed
- Cashback amount earned
- Customer phone (masked: `****1234`)

**Failure:**

If validation fails, you'll see error dialog:
- Bill too low (< 2 JOD)
- Duplicate bill
- Customer blacklisted
- Security PIN lock triggered

---

## Security Features for Auto-Trigger

### 1. All Validations Apply

Auto-triggered transactions go through **same security checks** as manual transactions:

- ✅ Bill amount validation (minimum 2.00 JOD)
- ✅ Duplicate bill detection
- ✅ Customer blacklist check
- ✅ **PIN lock security check** (1 redemption per 12 hours)
- ✅ Backend authentication validation

### 2. PIN Lock Can Block Auto-Flow

![Security PIN](../../images/screenshots/security-pin-dialog.png)

**Important**: If customer has processed 1+ transaction within 12 hours, PIN lock will trigger even for auto-triggered transactions.

**What happens:**
1. Backend sends auto-trigger
2. ILY Cash detects redemption limit reached
3. Red security PIN dialog appears (blocks auto-submit)
4. **You must enter PIN: 2941** to continue
5. Transaction proceeds after correct PIN

**This is intentional** - prevents fraud even in automated flow.

### 3. Fraud Detection

**Blacklist Check:**

![Fraud Alert](../../images/screenshots/fraud-alert-blacklist.png)

If customer is blacklisted:
- Orange warning dialog appears
- Transaction is blocked
- You cannot override
- Customer must contact ILY support

---

## Monitoring Auto-Trigger Activity

### Console Logs

To see detailed auto-trigger activity, run app from command line:

```cmd
cd C:\Program Files\ILY Cash
ILY_Cash.exe
```

**Look for these log patterns:**

**WebSocket Connection:**
```
✅ WebSocket connected to channel-12345
🔌 Ably connection state: connected
```

**Incoming Message:**
```
📩 Message received on channel-12345
🚀 Auto-flow started for E-0791234567
```

**Processing:**
```
🔒 Security check: PASSED (0 redemptions in 12h)
💾 RECORDING REDEMPTION for 0791234567 at 2025-01-15 14:30:22
✅ Transaction completed successfully
```

**Security Block:**
```
🔒 Security check: FAILED (1 redemption in 12h)
🔐 PIN lock triggered for 0791234567
⏸️ Auto-flow paused, waiting for PIN entry
```

### Home Screen Status Indicator

**Server Status** (top bar):
- **Green dot**: WebSocket connected, auto-trigger active
- **Red dot**: WebSocket disconnected, auto-trigger won't work
- **Yellow dot**: Connecting...

---

## Troubleshooting Auto-Trigger Issues

### Issue 1: Auto-Trigger Not Working

**Symptoms:**
- No notification when backend sends transaction
- App doesn't auto-navigate
- Manual transactions work fine

**Solutions:**

**Check 1: ILY Cash is Running**
```
✅ Check Windows system tray for ILY icon
✅ If not visible, launch ILY_Cash.exe
❌ App cannot receive WebSocket messages when closed
```

**Check 2: WebSocket Connected**
```
✅ Look at home screen top bar
✅ Green indicator = WebSocket connected
❌ Red indicator = Connection failed
```

**Check 3: Logged In**
```
✅ You must be logged in for WebSocket to connect
❌ WebSocket disconnects on logout
```

**Check 4: Internet Connection**
```cmd
ping cashback-api.meeza.app

# Should show responses, not "Request timed out"
```

**Check 5: Console Logs**

Run from command line and look for:
```
✅ WebSocket connected
📩 Message received
🚀 Auto-flow started
```

If you see:
```
❌ WebSocket connection failed
🔌 Ably error: Connection timeout
```

Solution: Check internet connection, verify backend is running

---

### Issue 2: Auto-Trigger Shows Error Every Time

**Symptoms:**
- Auto-trigger works (notification appears)
- But always shows error dialog
- Specific error message appears

**Common Errors:**

**Error: "Bill amount too low"**

![Error Low](../../images/screenshots/error-bill-too-low-arabic.png)

**Cause**: Backend sent bill value < 2.00 JOD

**Solution**:
- Contact backend team
- Verify WhatsApp message format
- Check backend validation rules

---

**Error: "Duplicate bill number"**

![Duplicate](../../images/screenshots/error-duplicate-bill.png)

**Cause**: Bill number already processed in last 30 days

**Solution**:
- Bill was already submitted (manually or auto)
- Check transaction history
- Verify bill number is correct

---

**Error: "Customer blacklisted"**

![Blacklist](../../images/screenshots/fraud-alert-blacklist.png)

**Cause**: Backend marked customer as fraudulent

**Solution**:
- DO NOT process transaction
- Customer must contact ILY support
- Cannot be overridden

---

### Issue 3: PIN Lock Blocks Every Auto-Trigger

**Symptoms:**
- Auto-trigger notification appears
- Security PIN dialog shows immediately
- Must enter PIN 2941 for every transaction

**Cause**: Customer has processed 1+ transaction within 12 hours

**Solutions:**

**Solution 1: Enter PIN (Correct)**
```
1. Red security dialog appears
2. Enter PIN: 2941
3. Click "Unlock"
4. Transaction proceeds
5. Counter resets to 0
```

**Solution 2: Adjust Security Settings (Administrators Only)**

File: `lib/config/security_config.dart`

```dart
// Current settings
static const int maxRedemptionsBeforeLock = 1;
static const int redemptionTimeWindowHours = 12;

// To allow more transactions:
static const int maxRedemptionsBeforeLock = 5; // Allow 5 transactions
```

**Warning**: Lowering security increases fraud risk.

---

### Issue 4: Monitoring Phone Not Triggering

**Symptoms:**
- Monitoring phone (0790000001) should bypass security
- But PIN lock still triggers
- Manual entry works without PIN

**Cause**: Whitelist check may not apply to auto-trigger flow

**Configuration**:

File: `assets/config.json`

```json
{
  "monitoring_phone": "0790000001"
}
```

Verify monitoring phone is correctly configured.

**Debug**:

Run from console and look for:
```
🔒 Security check for 0790000001
✅ Monitoring phone detected - bypassing security
```

If you see:
```
🔒 Security check for 0790000001
🔐 PIN lock triggered
```

Contact technical support - whitelist not working for auto-trigger.

---

## Technical Details

### WebSocket Service

**File**: `lib/services/websocket_service.dart`

**Connection Lifecycle:**
1. **Login** → WebSocket connects with branch ID
2. **Subscribe** to `channel-{branchId}`
3. **Listen** for messages
4. **Trigger** AutoFlowService on message
5. **Logout** → WebSocket disconnects

**Reconnection Logic:**
- Auto-reconnects on connection drop
- Exponential backoff: 1s, 2s, 4s, 8s...
- Max retry: 5 attempts

**Message Format** (from backend):
```json
{
  "type": "earn_cashback",
  "phone": "0791234567",
  "billNumber": "INV-123",
  "billValue": "25.50",
  "fullName": "Customer Name",
  "otp": "1234"
}
```

### Auto-Flow Service

**File**: `lib/services/auto_flow_service.dart`

**Processing Steps:**
1. Receive WebSocket message
2. Parse transaction data
3. Add `E-` prefix to phone number
4. Navigate to Earn Cashback page
5. Pre-fill form with data
6. Run security checks
7. Auto-submit transaction
8. Show result dialog

**Prefix Handling:**

```dart
// In earn_cashback_page.dart
final phone = arguments['phone'] as String;
final isAutoTrigger = phone.startsWith('E-');
```

The `E-` prefix tells the app this is an auto-triggered transaction from backend.

### Security Check Integration

**File**: `lib/pages/earn_cashback/earn_cashback_page.dart` (lines 178-184)

**Critical Security Flow:**

```dart
// Security check runs for ALL transactions (manual and auto)
if (shouldTriggerSecurity && !isMonitoringPhone) {
  // Show PIN lock dialog
  final pinResult = await _showSecurityPinDialog();
  if (pinResult != true) {
    return; // Block transaction if PIN incorrect
  }
}
```

**This was a bug fix** - previously auto-trigger bypassed security. Now fixed to run security for all transactions.

---

## Configuration Options

### For Administrators

**WebSocket Configuration:**

No configuration file - hardcoded in `websocket_service.dart`:
- Ably endpoint: `wss://realtime.ably.io`
- Channel pattern: `channel-{branchId}`

**Auto-Trigger Behavior:**

File: `assets/config.json`

```json
{
  "monitoring_phone": "0790000001",
  "auto_submit_timer_minutes": 15
}
```

**Security Settings:**

File: `lib/config/security_config.dart`

```dart
static const int maxRedemptionsBeforeLock = 1;
static const int redemptionTimeWindowHours = 12;
static const String securityPin = "2941";
```

**Important**: To change PIN, modify securityPin constant and rebuild app.

---

## Comparison: Manual vs Auto-Trigger

| Aspect | Manual Entry | Auto-Trigger (WhatsApp) |
|--------|-------------|------------------------|
| **Initiation** | User clicks "Earn Cashback" | Backend sends WebSocket message |
| **Phone Format** | `0791234567` | `E-0791234567` |
| **Form Input** | Manual typing | Pre-filled from backend |
| **Speed** | 30-60 seconds | 5-10 seconds |
| **Security Checks** | All validations apply | **All validations apply** (PIN lock, fraud, duplicate) |
| **User Attention** | Full attention required | Minimal attention (just verify success) |
| **Best For** | Single transactions, learning | High-volume processing, efficiency |

---

## Best Practices

### 1. Keep ILY Cash Running

**Recommendation**: Minimize to system tray, don't close

**Why**: WebSocket only receives messages when app is running

**How**:
1. Click minimize (not close)
2. App goes to system tray
3. Right-click icon → "Keep running in background"

---

### 2. Monitor WebSocket Status

**Check before busy periods:**
- Look at home screen status indicator
- Green dot = ready for auto-trigger
- Red dot = reconnect (logout → login)

**If red**:
1. Logout
2. Login again
3. WebSocket reconnects automatically
4. Status turns green

---

### 3. Watch for Notifications

**When notification appears:**
1. Don't ignore it
2. Watch for result dialog
3. Verify success before continuing
4. Check for errors (PIN lock, fraud, duplicate)

---

### 4. Enter PIN Quickly When Required

**If security PIN appears:**
- Enter 2941 immediately
- Transaction has short timeout
- Don't let it sit too long

---

### 5. Review Console Logs Periodically

**For administrators:**

Run from command line once per week:
```cmd
ILY_Cash.exe
```

Look for patterns:
- Frequent PIN locks → may need to adjust security threshold
- Frequent fraud alerts → review customer verification process
- WebSocket disconnections → check network stability

---

## Common Questions

**Q: Can I disable auto-trigger?**
A: No, auto-trigger is always active when logged in. It's a core feature.

**Q: Why does auto-trigger ask for PIN?**
A: Security check detected customer has processed transactions recently. This is fraud prevention.

**Q: Can I adjust the 12-hour security window?**
A: Yes, administrators can modify `redemptionTimeWindowHours` in `security_config.dart` and rebuild.

**Q: What if backend sends incorrect data?**
A: ILY Cash validates all data. If invalid, error dialog appears and transaction is blocked.

**Q: Can I process auto-trigger manually instead?**
A: No, when backend sends via WebSocket, it auto-processes. You cannot intercept or manually review before submission.

**Q: Does monitoring phone (0790000001) bypass PIN lock for auto-trigger?**
A: It should, but verify in console logs. Look for "Monitoring phone detected - bypassing security" message.

---

## Troubleshooting Checklist

Use this checklist to diagnose auto-trigger issues:

```
Auto-Trigger Not Working?

[ ] ILY Cash is running (check system tray)
[ ] WebSocket connected (green indicator on home screen)
[ ] Logged in (WebSocket requires active session)
[ ] Internet connected (ping cashback-api.meeza.app)
[ ] Backend is sending messages (verify with backend team)

Auto-Trigger Shows Errors?

[ ] Check error message type (bill too low, duplicate, fraud)
[ ] Verify backend sent correct data format
[ ] Check if security PIN lock is blocking (enter 2941)
[ ] Review customer transaction history for duplicates

Auto-Trigger Successful But Slow?

[ ] Check internet speed (high latency affects WebSocket)
[ ] Monitor console logs for delays in processing
[ ] Verify backend is sending immediately (not delayed)

Security PIN Appears Too Often?

[ ] Review redemption history for customer
[ ] Check security settings in security_config.dart
[ ] Verify customer is not processing multiple transactions rapidly
```

---

## Related Documentation

- **[Process Cashback (Manual)](process-cashback.md)** - Manual transaction flow
- **[Troubleshooting Guide](../troubleshooting/common-errors.md)** - All error scenarios
- **[Getting Started](../getting-started/README.md)** - Home screen and status indicators
- **[Security System](../technical/security.md)** - PIN lock and fraud detection details

---

## Technical Support

**For Issues:**
- **Auto-trigger not receiving**: Check WebSocket connection, verify backend integration
- **PIN lock blocking**: Enter 2941, review security_config.dart settings
- **Fraud alerts**: Customer must contact ILY support
- **Configuration changes**: Administrators can modify config.json and security_config.dart

**Debug Mode:**

Run from command line for detailed logs:
```cmd
C:\Program Files\ILY Cash\ILY_Cash.exe
```

Monitor output for WebSocket events, security checks, and transaction processing.

---

**Auto-trigger system documentation complete.** For FAQ and general questions, see [FAQ section](../faq/general.md).
