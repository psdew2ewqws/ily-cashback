# Troubleshooting Common Errors

Complete solutions guide for all ILY Cash issues and errors.

---

## Login Issues

### Error: Cannot Login - Credentials Invalid

**Error Message**: "Invalid phone number or password"

**Solutions:**

1. **Verify phone number format**
   - Must be: 10 digits starting with 07
   - Example: `0791234567`
   - Don't include: +962, spaces, dashes

2. **Password is case-sensitive**
   - Check CAPS LOCK is off
   - Verify spelling carefully

3. **Contact system administrator**
   - Account may not be activated
   - Password may need to be reset
   - Confirm you're registered as merchant

**See**: ![Login Screen](../../images/screenshots/login-screen.png)

---

### Error: Connection Failed During Login

**Symptoms**: Login button clicked but nothing happens, or connection error appears

**Solutions:**

1. **Check internet connection**
   - Open browser, test connectivity
   - Verify WiFi/Ethernet connected

2. **Check server status**
   - Backend may be temporarily down
   - Wait 1-2 minutes and retry

3. **Firewall blocking app**
   - Windows Settings → Security → Firewall
   - Allow ILY_Cash.exe through firewall

4. **Restart the app**
   - Close completely (check system tray)
   - Relaunch and try again

---

## Cashback Processing Errors

### Error: Bill Amount Too Low (< 2.00 JOD)

![Error: Bill Too Low](../../images/screenshots/error-bill-too-low-arabic.png)

**Error Message** (Arabic):
> خطأ في العملية
> قيمة الفاتورة يجب أن تكون 2.00 دينار على الأقل

**Translation**: "Error in operation - Bill value must be at least 2.00 JOD"

**Cause**: Minimum bill amount is 2.00 JOD (enforced by system)

**Solutions:**

1. **Verify amount entered**
   - Check for typos
   - Example: `2.00` not `0.20`

2. **Use decimal point** (not comma)
   - ✅ Correct: `2.50`
   - ❌ Wrong: `2,50`

3. **If bill is actually below 2 JOD**
   - Customer doesn't qualify for cashback
   - Inform customer of minimum requirement
   - Do not attempt to process

**Why 2.00 JOD minimum**: Business policy to ensure meaningful transactions

---

### Error: Duplicate Bill Number (Fraud Detection)

![Error: Duplicate Bill](../../images/screenshots/error-duplicate-bill.png)

**Error Message** (Red alert):
> Old Receipt - Cannot Be Redeemed
> **FRAUD ATTEMPT DETECTED**

**Details shown**:
- Bill number that was already submitted
- Date and time of original redemption
- Warning that this may be fraud

**Cause**: Bill number already processed (within last 30 days)

**Solutions:**

1. **DO NOT override** this warning
2. **Verify bill number carefully**
   - Check for typos
   - Compare with physical receipt
3. **If bill is truly new**
   - Contact ILY support
   - Do NOT process manually
4. **If customer claims they didn't redeem before**
   - Politely refer to ILY support
   - Do not argue or accuse
5. **Possible causes**:
   - Customer trying to redeem twice (fraud)
   - Accidental duplicate entry
   - Bill number coincidentally matches old one

**Why this is critical**: Prevents customers from redeeming same receipt multiple times (double-dipping fraud)

**System behavior**: Bill numbers tracked for 30 days in `submitted_bill_numbers` set

---

### Error: Customer Blacklisted (Fraud Alert)

![Fraud Alert: Blacklist](../../images/screenshots/fraud-alert-blacklist.png)

**Error Message** (Orange warning):
> Customer Blocked
> Phone: ****1932

**Cause**: Customer's phone number is on backend fraud blacklist

**What this means**:
- Customer flagged for fraudulent activity by ILY system
- Backend has blocked this account
- This protects your store from fraud losses

**What to do**:

1. **DO NOT PROCESS** the transaction
2. **DO NOT bypass** this warning
3. **DO NOT argue** with customer
4. **Politely inform customer**:
   - "There's an issue with your account"
   - "Please contact ILY support for assistance"
   - Do NOT mention "fraud" or "blacklist" explicitly
5. **Click "Close"** to return to home
6. **Report to manager** if customer becomes aggressive

**If customer insists**:
- Remain professional and calm
- Repeat: "Please contact ILY support"
- Do not process manually under any circumstances
- This system protects your business

**Security note**: This is a **backend blacklist** maintained by ILY - separate from local PIN lock system

---

### Error: Security PIN Lock Triggered

![Security PIN Dialog](../../images/screenshots/security-pin-dialog.png)

**Security Alert displays**:
> **SECURITY ALERT**
> Suspicious activity detected
> Enter security PIN to continue

**Cause**: Same phone number has processed 2+ transactions within 12 hours

**Why it triggers**:
- Fraud prevention system
- Detects unusual redemption patterns
- Configured to allow 1 transaction per 12 hours per phone number
- Prevents customers from abusing the system

**What to do**:

1. **Red security dialog appears** (cannot be dismissed)
2. **Enter security PIN**: `2941`
   - Contact admin if you don't have PIN
   - Default PIN: 2941 (may be different for your installation)
3. **Click "Unlock"**
4. **If PIN is correct**:
   - Transaction proceeds normally
   - Redemption counter resets to 0
   - Customer history cleared
5. **If PIN is incorrect**:
   - Transaction remains blocked
   - Must enter correct PIN to continue
   - No bypass available

**When to be concerned**:
- Customer trying multiple redemptions quickly
- Possible organized fraud attempt
- May need to escalate to manager
- Consider reporting suspicious behavior

**Configuration** (for administrators):
- File: `lib/config/security_config.dart`
- Max redemptions before lock: 1
- Time window: 12 hours
- Security PIN: 2941 (hardcoded)

**Exception**: Monitoring phone (whitelisted) never triggers PIN lock

---

### Error: Customer Not Found / Not Registered

**Error Message**: "Customer not registered in ILY system"

**Cause**: Phone number not in ILY database

**Solutions:**

1. **Double-check phone number**
   - Ask customer to verify
   - Common mistake: wrong digit or transposed numbers

2. **Try alternative formats**
   - ✅ `0791234567` (correct)
   - ❌ `+962791234567` (don't include country code)
   - ❌ `791234567` (missing leading 0)

3. **Ask for alternative number**
   - Customer may have multiple phones
   - Registered with different number

4. **Customer needs to register**
   - Direct to ILY mobile app or website
   - Registration required before earning cashback
   - Takes 2-3 minutes to complete

5. **Wait for account activation**
   - New registrations may take 5-10 minutes to sync
   - Ask customer to retry later

---

### Error: Invalid OTP Code

**Error Message**: "OTP code is incorrect or expired"

**Cause**: Wrong OTP entered or OTP expired (>5 minutes old)

**Solutions:**

1. **Ask customer to check SMS again**
   - Verify they're reading correct code
   - Check SMS timestamp

2. **Request new OTP**
   - Go back to phone entry
   - Re-enter phone number
   - New OTP will be sent

3. **Check SMS delays**
   - SMS may take 5-30 seconds to arrive
   - Wait before assuming it failed

4. **Verify phone number is correct**
   - OTP sent to entered number
   - If wrong number entered, customer won't receive it

**OTP Rules**:
- **Format**: 4 digits
- **Expiry**: 5 minutes after generation
- **Single use**: Each OTP can only be used once
- **New request**: Previous OTP becomes invalid when new one requested

---

## Network and Connection Errors

### Error: Cannot Connect to Server

**Error Message**: "Connection failed" or "Server unreachable"

**Causes**: Internet connection issue, backend server down, or firewall blocking

**Solutions:**

**1. Check Internet Connection**
```
Open browser → Test connectivity → If fails, restart router
```

**2. Check Server Status Indicator**
- Look at top of ILY Cash window
- 🟢 Green = Connected
- 🔴 Red = Disconnected
- If red: Backend may be down

**3. Test Network**
```bash
# Open Command Prompt and run:
ping cashback-api.meeza.app
```
- If ping fails: Network issue
- If ping succeeds: Firewall may be blocking app

**4. Check Windows Firewall**
1. Windows Settings → Update & Security → Windows Security
2. Firewall & network protection
3. Allow an app through firewall
4. Find "ILY_Cash" or "ILY Cash"
5. Check both Private and Public boxes
6. Click OK and restart app

**5. Restart App**
- Close ILY Cash completely
- Check system tray to ensure fully closed
- Relaunch and login

**6. Wait and Retry**
- Backend may be temporarily overloaded
- Wait 2-3 minutes
- Try transaction again

---

### Error: Request Timeout

**Error Message**: "Request timed out" or "Server not responding"

**Cause**: Slow internet or backend server overloaded

**Solutions:**

1. **Check internet speed**
   - Run speed test (speedtest.net)
   - Minimum recommended: 1 Mbps

2. **Close bandwidth-heavy apps**
   - Stop downloads/uploads
   - Close streaming apps
   - Free up network

3. **Wait and retry**
   - Backend may be busy
   - Wait 30-60 seconds
   - Try again

4. **Restart app**
   - Complete close and reopen
   - Fresh connection established

**Timeout configurations**:
- HTTP requests: 30 seconds
- WebSocket: 10 seconds
- Storage operations: 5 seconds

---

## WhatsApp Auto-Trigger Issues

### Issue: WhatsApp Messages Not Auto-Triggering

**Problem**: Sending message via WhatsApp but ILY Cash doesn't respond

**Solutions:**

**1. Verify ILY Cash is Running**
- Check Windows system tray for ILY icon
- App must be running (can be minimized)
- If closed, auto-detection won't work

**2. Check WebSocket Connection**
- Green status indicator = WebSocket connected
- Red status indicator = Not connected, won't receive messages
- If red: Restart app to reconnect

**3. Verify Message Format**
From backend, messages must include phone in correct format

**4. Check Ably Configuration**
- WebSocket connected to: `wss://realtime.ably.io`
- Channel: `channel-{branchId}`
- API key configured (hardcoded in app)

**5. Monitor Console Logs**
Run app from command line to see WebSocket activity:
```
C:\path\to\ILY_Cash.exe
```
Look for:
- `✅ WebSocket connected`
- `📩 Message received`
- `🚀 Auto-flow started`

**6. Check Security Dialog Not Blocking**
- If PIN lock dialog is showing, auto-flow blocked
- Complete or dismiss PIN dialog first

**System Requirements**:
- Active internet connection
- WebSocket port not blocked (443)
- Ably servers reachable

---

## Bill Auto-Submit Issues

### Issue: Bills from POS Not Auto-Submitting

**Problem**: Bill detected at `C:\ily\extracted.txt` but not auto-submitted

**Causes & Solutions:**

**1. Check File Exists**
```
C:\ily\extracted.txt
```
- File must exist at this exact path
- Format: `{billNumber},{billValue}` (e.g., "100263,1.852")

**2. Verify Timer is Running**
Check console for:
```
💾 Bill tracked: 100263
💾 15-minute timer started
```

**3. Check if Manually Processed**
- If you process the bill manually within 15 minutes
- Timer cancels automatically
- Bill marked as processed

**4. Verify Monitoring Phone**
- Auto-submit uses monitoring phone from config
- Default: `0790000001`
- Check `assets/config.json`

**5. Check Timer Configuration**
```json
{
  "auto_submit_timer_minutes": 15
}
```
- Default: 15 minutes
- Can be configured in config.json

**6. Console Logs to Watch**:
```
💾 Bill tracked: {billNumber}
💾 Timer expired - auto-submitting
💾 Auto-submit success
```

---

## Update Issues

### Issue: Auto-Update Not Working

**Problem**: New version available but not downloading/installing

**Solutions:**

**1. Check Update Schedule**
- **Immediate check**: 10 seconds after app starts
- **Daily check**: Every day at 11:00 AM (Jordan Time / UTC+3)

**2. Verify Internet Connection**
- Updates download from GitHub
- Requires stable internet
- Test by opening browser

**3. Check GitHub Access**
Update URL: `https://raw.githubusercontent.com/psdew2ewqws/ily-cashback/main/appcast.xml`
- Try opening this URL in browser
- If blocked: Network/firewall issue

**4. Manual Update**
1. Go to https://github.com/psdew2ewqws/ily-cashback/releases
2. Download latest version ZIP
3. Close ILY Cash
4. Extract and replace files
5. Run new version

**5. Check WinSparkle Service**
- Auto-update powered by WinSparkle
- No admin privileges required
- Updates install silently

**6. Firewall Blocking Downloads**
- Windows may block GitHub
- Temporarily disable Windows Defender
- Try update again
- Re-enable after update

---

## App Performance Issues

### Issue: App Running Slow or Lagging

**Solutions:**

1. **Restart App**
   - Close completely (check system tray)
   - Reopen

2. **Check System Resources**
   - Open Task Manager (Ctrl+Shift+Esc)
   - Check CPU and RAM usage
   - Close unnecessary programs

3. **Restart Computer**
   - Simple but effective
   - Clears memory leaks

4. **Check for Updates**
   - Performance improvements in newer versions
   - Install latest update

5. **Reinstall App**
   - Download fresh copy
   - Uninstall current version
   - Install new copy

---

### Issue: App Crashes or Freezes

**Solutions:**

**1. Force Close**
```
Press Ctrl + Alt + Delete
→ Task Manager
→ Find "ILY_Cash.exe"
→ Click "End Task"
```

**2. Check Error Logs**
Run app from command prompt to see crash logs:
```cmd
cd C:\path\to\ILY_Cash
ILY_Cash.exe
```

**3. Update to Latest Version**
- Bugs may be fixed in newer releases
- Check GitHub releases

**4. Check System Requirements**
- Windows 10 or later
- 4 GB RAM minimum
- Sufficient disk space

**5. Reinstall**
- Uninstall current version
- Download latest from GitHub
- Fresh installation

---

## Printing Issues

### Issue: Receipts Not Printing (Burn Cashback)

**Problem**: Receipt should print after burn transaction but doesn't

**Solutions:**

**1. Check Printer Connection**
- USB cable securely connected
- Printer powered on
- Windows recognizes printer

**2. Set Default Printer**
```
Windows Settings
→ Devices
→ Printers & scanners
→ Select your thermal printer
→ Set as default
```

**3. Test Printer**
```
Right-click printer
→ Printing preferences
→ Print test page
```
If test page prints, ILY Cash should work

**4. Check Printer Drivers**
- Ensure drivers installed
- Update to latest drivers
- Restart computer after installing

**5. Verify Thermal Printer Compatibility**
- App uses standard USB thermal printers
- ESC/POS compatible printers recommended

**6. Check Print Queue**
```
Open printer queue
→ Clear any stuck jobs
→ Try printing again
```

---

## Security and Fraud Prevention

### Understanding Security Features

**1. Duplicate Bill Detection**
- Tracks all submitted bill numbers for 30 days
- Prevents same receipt from being redeemed twice
- Stored in SharedPreferences locally

**2. Suspicious Activity Detection**
- Monitors redemption frequency per phone number
- Triggers PIN lock after threshold
- Default: 1 redemption per 12 hours

**3. Backend Blacklist**
- Centrally maintained by ILY system
- Blocks known fraudulent accounts
- Cannot be overridden locally

**4. Whitelist System**
- Monitoring phone exempt from all checks
- Used for testing and auto-submit
- Default: 0790000001

**5. Bill Expiry Check**
- Backend validates bill is not too old
- Prevents redemption of ancient receipts

---

## Getting Help

### When to Contact Support

Contact ILY support team if:
- ✅ Login fails repeatedly after trying solutions
- ✅ Connection errors persist for >1 hour
- ✅ App crashes frequently (>3 times/day)
- ✅ Customer account issues (not found, blacklisted)
- ✅ Fraud alert questions or concerns
- ✅ PIN lock appearing too frequently
- ✅ WebSocket/auto-trigger not working

### Before Contacting Support

**Collect this information**:
1. Your merchant phone number
2. Branch ID (if known)
3. App version (check bottom of login screen)
4. Screenshot of error message
5. Steps that caused the problem
6. Console logs (if admin)

### How to Contact Support

**GitHub Issues** (for technical bugs):
https://github.com/psdew2ewqws/ily-cashback/issues

**System Administrator**:
- Contact your local IT support first
- They have access to backend and logs

---

## Quick Troubleshooting Checklist

When something goes wrong, try these in order:

1. ☐ **Check internet connection** - Open browser, test
2. ☐ **Check server status indicator** - Green = OK, Red = Issue
3. ☐ **Restart ILY Cash** - Close completely, reopen
4. ☐ **Check for updates** - Install latest version
5. ☐ **Clear and retry** - Start transaction from beginning
6. ☐ **Restart computer** - Simple but often effective
7. ☐ **Check Windows firewall** - Allow ILY Cash
8. ☐ **Wait 5 minutes and retry** - Temporary issues often resolve
9. ☐ **Contact support** - If problem persists

---

## Prevention Tips

**Avoid problems before they happen:**

1. **Keep app updated**
   - Auto-updates enabled by default
   - Install updates when prompted

2. **Use stable internet**
   - Wired connection preferred
   - Strong WiFi signal

3. **Restart app daily**
   - Fresh start prevents memory leaks
   - Clears cached issues

4. **Double-check all inputs**
   - Phone numbers (most common error)
   - Bill amounts (decimal points)
   - Bill numbers (typos cause duplicates)

5. **Monitor security alerts**
   - Never ignore blacklist warnings
   - Report suspicious patterns

6. **Keep Windows updated**
   - Security patches important
   - Driver updates help compatibility

7. **Regular backups**
   - Transaction history
   - Configuration settings

---

## Advanced Troubleshooting (For Administrators)

### Enable Debug Console

Run app from command line to see detailed logs:
```cmd
cd C:\Program Files\ILY Cash
ILY_Cash.exe
```

**Look for these log prefixes**:
- `🔒` - Security check logs
- `💾` - Redemption recording logs
- `📩` - WebSocket message logs
- `⚠️` - Warning messages
- `❌` - Error messages

### Check SharedPreferences Data

**Location**: `%APPDATA%\ILY Cash\shared_preferences\`

**Key files**:
- `redemption_history_by_phone` - Fraud detection data
- `submitted_bill_numbers` - Duplicate detection
- `token` - Authentication token

### Reset Security Data

**If PIN lock issues persist**:
1. Close ILY Cash
2. Delete: `%APPDATA%\ILY Cash\shared_preferences\redemption_history_by_phone`
3. Reopen app
4. Redemption history cleared

**Warning**: This resets fraud detection. Use with caution.

---

## Related Documentation

- [Getting Started Guide](../getting-started/README.md)
- [How to Process Cashback](../how-to/process-cashback.md)
- [WhatsApp Integration](../how-to/whatsapp-integration.md)
- [Technical Architecture](../technical/architecture.md)
- [Security System](../technical/security.md)

---

**Remember**: Most errors are temporary. Restart the app and try again. If problem persists, contact support with detailed information.
