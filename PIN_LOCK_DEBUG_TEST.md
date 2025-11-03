# 🔒 PIN Lock Debug Testing Guide

## ⚠️ CRITICAL BUG: Pin lock not triggering after 2 transactions

I've added comprehensive debug logging to track exactly what's happening.

---

## 🎯 Current Configuration

**From [security_config.dart](../lib/config/security_config.dart):**
- `maxRedemptionsBeforeLock = 1` → Allow 1 transaction, block the 2nd
- `suspiciousTimeWindowHours = 12` → Within 12 hours
- `securityPin = 2941` → PIN to unlock

**Expected Behavior:**
1. **Transaction 1** (any phone): ✅ ALLOWED (first time)
2. **Transaction 2** (same phone, within 12 hours): 🔒 **SHOULD TRIGGER PIN LOCK**
3. Enter PIN 2941 → Unlock and reset counter

---

## 📝 How to Test (Step-by-Step)

### Prerequisites
1. **Clear previous data** (optional but recommended):
   - Close ILY Cash if running
   - Delete SharedPreferences (or just run 2 transactions to test)

2. **Run the app from command line** to see console output:
   ```
   C:\Users\MSI\Desktop\IlyCashbackProject\cashback_app\cashback_app\build\windows\x64\runner\Debug\ILY_Cash.exe
   ```

3. **Keep the console window visible** - you'll see debug logs there

---

### Test Procedure

#### Transaction 1 (Should ALLOW):

1. **Open Earn Cashback**
2. **Fill in:**
   - Phone: `0791234567` (or any valid customer)
   - Bill: `TEST001`
   - Amount: `5.00`
3. **Click Submit**

**Expected Console Output:**
```
🔒━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔒 SECURITY CHECK: Starting suspicious activity check
🔒 Phone number (raw): 0791234567
🔒 Phone number (clean): 0791234567
🔒 Monitoring phone (whitelist): 0788393918
🔒 History JSON exists: false
🔒 ✅ NO HISTORY: First transaction for any phone - allowed
🔒━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

... (transaction proceeds) ...

💾━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
💾 RECORDING REDEMPTION
💾 Phone (raw): 0791234567
💾 Bill: TEST001
💾 Phone (clean): 0791234567
💾 Monitoring phone (whitelist): 0788393918
💾 Existing history: NO
💾 Previous redemptions for this phone: 0
💾 Adding redemption:
💾   Bill: TEST001
💾   Time: 2025-11-03T07:30:00.000Z
💾 ✅ SAVED: Total redemptions for this phone now: 1
💾━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

✅ **Transaction 1 should succeed**

---

#### Transaction 2 (Should TRIGGER PIN LOCK):

1. **Immediately do another transaction** (within 12 hours)
2. **Fill in:**
   - Phone: `0791234567` (SAME phone as transaction 1!)
   - Bill: `TEST002` (different bill number)
   - Amount: `5.00`
3. **Click Submit**

**Expected Console Output:**
```
🔒━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔒 SECURITY CHECK: Starting suspicious activity check
🔒 Phone number (raw): 0791234567
🔒 Phone number (clean): 0791234567
🔒 Monitoring phone (whitelist): 0788393918
🔒 History JSON exists: YES
🔒 Total redemptions in history for this phone: 1
🔒 Time window: 12 hours
🔒 Cutoff time: 2025-11-02T19:30:00.000Z
🔒 Current time: 2025-11-03T07:31:00.000Z
🔒   → Bill: TEST001, Time: 2025-11-03T07:30:00.000Z
🔒      ✓ WITHIN WINDOW (count: 1)
🔒 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔒 DECISION CRITERIA:
🔒   Recent redemptions: 1
🔒   Max allowed: 1
🔒   Check: 1 >= 1
🔒 ❌ SUSPICIOUS: Triggering PIN lock!
🔒   Recent bills: [TEST001]
🔒━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

🔒 **PIN dialog should appear!**

---

## 🧪 What to Check

### If PIN Lock WORKS:
- ✅ You see "🔒 ❌ SUSPICIOUS: Triggering PIN lock!" in console
- ✅ PIN dialog appears on screen
- ✅ Enter PIN 2941 to unlock
- ✅ Transaction proceeds after correct PIN

### If PIN Lock FAILS (Current Bug):
Look for these issues in the console:

**Issue 1: Security check not running at all**
- Console shows NO 🔒 logs
- **Cause**: Security check not being called

**Issue 2: Security check returns false (allowed)**
- Console shows 🔒 logs but says "✅ ALLOWED"
- Check the "DECISION CRITERIA" section:
  - What is "Recent redemptions"?
  - What is "Max allowed"?
  - What is the check result?

**Issue 3: Check times out or errors**
- Console shows: "🔒 ❌ ERROR in security check"
- **Cause**: Exception or timeout in security check

**Issue 4: Monitoring phone whitelist**
- Console shows: "🔒 ✅ WHITELISTED: Monitoring phone - skipping security check"
- **Cause**: You're using the monitoring phone (0788393918)
- **Fix**: Use a DIFFERENT phone number

**Issue 5: Recording fails**
- Transaction 1 shows: "💾 ❌ ERROR recording redemption"
- **Cause**: Can't save to SharedPreferences
- Transaction 2 will always be allowed because history is empty

---

## 📊 Expected Flow Diagram

```
Transaction 1 (Phone: 0791234567):
┌─────────────────────────────────┐
│ 🔒 Security Check                │
│  - Check history for 0791234567  │
│  - Found: 0 transactions         │
│  - Decision: ALLOW (0 < 1)       │
└────────────┬────────────────────┘
             │ ALLOWED
             ▼
┌─────────────────────────────────┐
│ Process Transaction              │
│  - Submit to server              │
│  - Success!                      │
└────────────┬────────────────────┘
             │ SUCCESS
             ▼
┌─────────────────────────────────┐
│ 💾 Record Redemption             │
│  - Save: 0791234567, TEST001     │
│  - History now: 1 transaction    │
└─────────────────────────────────┘

Transaction 2 (Phone: 0791234567, within 12 hours):
┌─────────────────────────────────┐
│ 🔒 Security Check                │
│  - Check history for 0791234567  │
│  - Found: 1 transaction          │
│  - Within 12 hours? YES          │
│  - Decision: BLOCK (1 >= 1) ❌   │
└────────────┬────────────────────┘
             │ BLOCKED
             ▼
┌─────────────────────────────────┐
│ 🔒 PIN Lock Dialog               │
│  - Enter PIN: 2941               │
│  - Unlock app                    │
│  - Clear history                 │
└────────────┬────────────────────┘
             │ UNLOCKED
             ▼
┌─────────────────────────────────┐
│ Process Transaction              │
│  - Continue with transaction     │
└─────────────────────────────────┘
```

---

## 🔍 Root Cause Analysis

Based on logs, we can identify the issue:

### Scenario A: Whitelist Bug
**Symptom**: Console shows "WHITELISTED" for all transactions
**Cause**: Monitoring phone is hardcoded or misconfigured
**Fix**: Check ConfigHelper.getMonitoringPhone() value

### Scenario B: Recording Bug
**Symptom**: Transaction 2 shows "Previous redemptions: 0"
**Cause**: recordRedemption() is not saving data
**Fix**: Check SharedPreferences write permissions

### Scenario C: Timeout Bug
**Symptom**: Security check times out (3 second timeout)
**Cause**: SharedPreferences read is slow
**Fix**: Increase timeout or optimize check

### Scenario D: Logic Bug
**Symptom**: Check says "1 >= 1" is false
**Cause**: Wrong comparison operator
**Fix**: Verify comparison logic

---

## 🐛 How to Share Logs with Me

1. **Run the test** (2 transactions, same phone)
2. **Copy console output** from command window
3. **Look for** the 🔒 and 💾 sections
4. **Share the full output** so I can analyze

**Especially important:**
- The "DECISION CRITERIA" section from transaction 2
- Any error messages
- The "Recent redemptions" and "Max allowed" values

---

## 🔧 Quick Fixes to Try

### If monitoring phone whitelist is the issue:
```dart
// In lib/const/config_helper.dart
static String getMonitoringPhone() {
  return '0788393918'; // Make sure this is DIFFERENT from your test phone
}
```

### If you want to clear history manually:
Run this in Dart DevTools console while app is running:
```dart
await SharedPreferences.getInstance().then((prefs) => prefs.remove('redemption_history_by_phone'));
```

### If you want to test with more lenient settings:
Edit [lib/config/security_config.dart](../lib/config/security_config.dart):
```dart
static const int maxRedemptionsBeforeLock = 2; // Allow 2, block 3rd
static const int suspiciousTimeWindowHours = 1; // Shorter window for testing
```

---

## ✅ After Testing

**Send me:**
1. Console output from both transactions
2. Whether PIN dialog appeared or not
3. Any unexpected behavior

**I will:**
1. Analyze the logs
2. Identify the root cause
3. Fix the bug
4. Test the fix
5. Verify PIN lock works correctly

---

**Let's fix this critical security issue! 🔒**
