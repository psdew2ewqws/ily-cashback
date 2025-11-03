# General FAQ

Frequently asked questions about ILY Cash cashback system.

***

## Getting Started

### What is ILY Cash?

ILY Cash is a Windows desktop application that enables retail merchants in Jordan to:

* Award cashback points to customers (earn cashback)
* Allow customers to redeem accumulated points (burn cashback)
* Process transactions automatically via WhatsApp integration
* Monitor and prevent fraud with built-in security features

**Target users**: Retail employees at ILY partner stores

***

### What are the system requirements?

**Minimum Requirements:**

* Windows 10 or later (64-bit)
* 4 GB RAM
* 200 MB free storage
* Stable internet connection

**Recommended:**

* Windows 11
* 8 GB RAM or more
* High-speed internet (10 Mbps+)

See [System Requirements](../../../#system-requirements) for full details.

***

### How do I install ILY Cash?

1. Download the latest release from [GitHub Releases](https://github.com/psdew2ewqws/ily-cashback/releases)
2. Extract the ZIP file to `C:\Program Files\ILY Cash\`
3. Double-click `ILY_Cash.exe`
4. Click "Run anyway" if Windows shows security warning
5. Login with your employee credentials

See [Installation Guide](../../../#installation) for detailed steps.

***

### How do I get login credentials?

**Contact your system administrator** to receive:

* Employee phone number (10 digits, e.g., 0791234567)
* Password

You cannot create an account yourself - all accounts are managed by ILY administrators.

***

### Do I need admin privileges to run ILY Cash?

**No** - ILY Cash runs as a regular Windows application and does not require administrator privileges.

**Exception**: If you install to `C:\Program Files\`, Windows may ask for admin approval during installation (one-time only).

***

### Can I run ILY Cash on Mac or Linux?

**No** - ILY Cash is currently Windows-only. It requires Windows 10 or later (64-bit).

**Why**: The app uses Windows-specific features for printer integration, file monitoring, and auto-update system.

***

## Using ILY Cash

### What are "Earn" and "Burn" cashback?

**Earn Cashback** (Green Button):

* Customer makes a purchase
* You process their bill
* Customer earns cashback points
* Points added to their account balance

**Burn Cashback** (Red Button):

* Customer wants to redeem points
* You process the redemption
* Points deducted from their balance
* Customer receives discount or cash equivalent

See [Process Cashback Guide](../how-to/process-cashback.md) for detailed steps.

***

### What is the minimum bill amount?

**Minimum: 2.00 JOD**

Bills below 2.00 JOD cannot be processed and will show this error:

![Error Low](../../images/screenshots/error-bill-too-low-arabic.png)

**Why this limit**: Fraud prevention and transaction cost management.

***

### How long does it take to process a transaction?

**Manual Transaction**: 30-60 seconds

* Enter customer phone
* Fill form (OTP, bill number, amount, name)
* Submit and wait for confirmation

**Auto-Trigger (WhatsApp)**: 5-10 seconds

* Backend sends transaction data
* App auto-processes
* Result appears immediately

***

### Can I process multiple transactions at once?

**No** - ILY Cash processes transactions sequentially, one at a time.

**Reason**: Each transaction requires:

* Backend validation
* Fraud detection check
* Security PIN check (if applicable)
* Database update

**Tip**: Use WhatsApp auto-trigger for high-volume periods to speed up processing.

***

### What happens if I close the app during a transaction?

**During submission**: Transaction may fail, customer won't be charged/credited

**After success dialog**: Transaction is complete, safe to close

**Best practice**: Wait for success or error dialog before closing the app.

***

### Can I minimize ILY Cash to system tray?

**Yes** - Click minimize button (not close)

**Benefits**:

* App continues running in background
* Receives WhatsApp auto-trigger notifications
* WebSocket stays connected
* Ready to process transactions instantly

**Access**: Click ILY icon in Windows system tray to restore window

***

## WhatsApp Auto-Trigger

### What is WhatsApp auto-trigger?

A backend-driven system where transactions are automatically processed when the backend receives WhatsApp messages.

**Flow**:

```
Customer sends WhatsApp → Backend receives → Backend sends to app → App auto-processes
```

**You see**: Notification, auto-navigation to cashback page, auto-submit, result dialog

See [WhatsApp Integration Guide](../how-to/whatsapp-integration.md) for complete details.

***

### How do I know if a transaction is auto-triggered?

**Phone number starts with `E-` prefix**

Examples:

* Manual: `0791234567`
* Auto-triggered: `E-0791234567`

The `E-` indicates the transaction came from backend via WhatsApp.

***

### Do I need WhatsApp installed on my computer?

**No** - ILY Cash does NOT require WhatsApp to be installed.

**How it works**: Backend server handles WhatsApp integration and sends data to ILY Cash via WebSocket (internet connection).

***

### Can I disable auto-trigger?

**No** - Auto-trigger is always active when you're logged in.

**Why**: It's a core feature designed to improve processing efficiency during busy periods.

**Note**: You must keep the app running to receive auto-trigger notifications.

***

### Why does auto-trigger ask for PIN sometimes?

**Security check**: Customer has processed 1+ transaction within the last 12 hours.

**What to do**:

1. Red security PIN dialog appears
2. Enter PIN: **2941**
3. Click "Unlock"
4. Transaction proceeds

This is **fraud prevention** - even automated transactions are checked for suspicious activity.

See [Security System](../how-to/process-cashback.md#security-features) for details.

***

## Security and Fraud Prevention

### What is the security PIN lock?

A fraud prevention system that triggers when a customer has processed **1 or more transactions within 12 hours**.

**What happens**:

1. Red security dialog appears
2. You must enter PIN: **2941**
3. If correct, transaction proceeds and counter resets
4. If wrong, transaction is blocked

See [Security PIN Dialog](../how-to/process-cashback.md#step-7-security-pin-lock-if-triggered) for details.

***

### What is the security PIN?

**PIN: 2941**

**When needed**:

* Customer has processed multiple transactions within 12 hours
* Security check detects suspicious activity pattern

**Who knows it**: All authorized employees at your branch

**Can I change it**: Only administrators can change it by modifying app configuration and rebuilding.

***

### What does the orange fraud warning mean?

![Fraud Alert](../../images/screenshots/fraud-alert-blacklist.png)

**Customer is blacklisted** by ILY backend for fraudulent activity.

**What to do**:

* **DO NOT process the transaction**
* Transaction is automatically blocked (you cannot override)
* Customer must contact ILY support to resolve

**Why**: Backend has flagged this customer for fraud prevention.

***

### Can I override fraud warnings?

**No** - Fraud warnings (orange dialog) cannot be overridden.

Only ILY support can remove customers from the blacklist after verification.

***

### How does duplicate bill detection work?

ILY Cash tracks all bill numbers for **30 days**.

**If duplicate detected**:

![Duplicate](../../images/screenshots/error-duplicate-bill.png)

**Error message**: "Bill number already processed"

**What to do**:

1. Verify bill number is correct
2. Check if customer already submitted this bill
3. Check transaction history
4. Use different bill number if this is a new transaction

***

### What is the monitoring phone?

**Phone: 0790000001**

**Purpose**: Testing and administrative account that bypasses security checks

**Special behavior**:

* No PIN lock trigger
* No redemption limit
* Used for testing and demonstration

**Configuration**: Set in `assets/config.json`

***

## Errors and Troubleshooting

### I can't login - what should I check?

**Common issues**:

1. **Phone number format**: Must be 10 digits starting with 07
   * ✅ Correct: `0791234567`
   * ❌ Wrong: `+962791234567` or `791234567`
2. **Password incorrect**: Verify with administrator
3. **No internet connection**: Check network connectivity
4. **Backend server down**: Contact technical support

See [Login Troubleshooting](../troubleshooting/common-errors.md#login-issues) for all solutions.

***

### Transaction failed with "Network error" - what do I do?

**Immediate action**:

1. Check internet connection
2. Try the transaction again
3. If persists, restart app

**Verify connectivity**:

```cmd
ping cashback-api.meeza.app
```

Should show responses, not "Request timed out"

See [Network Errors](../troubleshooting/common-errors.md#network-and-connection-errors) for full troubleshooting.

***

### Error: "Bill amount too low" - but the amount is correct?

**Check decimal format**:

* ✅ Correct: `2.50` (period)
* ❌ Wrong: `2,50` (comma)

**Check minimum**:

* Must be ≥ 2.00 JOD
* 1.99 JOD will be rejected

**Arabic interface**: Error appears in Arabic with same meaning

![Error Low](../../images/screenshots/error-bill-too-low-arabic.png)

***

### Why do I keep seeing the security PIN dialog?

**Reason**: Same customer phone number has processed 1+ transaction within 12 hours

**Solution**:

* Enter PIN 2941 to continue
* This is fraud prevention (intentional behavior)
* Counter resets after you enter correct PIN

**If happening too often**: Contact administrator to review security settings

See [Security PIN Lock](../how-to/process-cashback.md#step-7-security-pin-lock-if-triggered) for details.

***

### The app is frozen or not responding - what do I do?

**Immediate steps**:

1. Wait 30 seconds (may be processing)
2. Check internet connection
3. If still frozen, close app (Task Manager if needed)
4. Restart ILY Cash
5. Login again

**Prevent freezing**:

* Keep Windows and app updated
* Close other resource-heavy programs
* Ensure stable internet connection

See [App Performance Issues](../troubleshooting/common-errors.md#app-performance-and-crashes) for details.

***

### How do I check if the app is running?

**Check system tray** (bottom right of Windows taskbar)

Look for **ILY Cash icon**

* ✅ Icon visible: App is running
* ❌ No icon: App is closed

**If not visible**: Launch `ILY_Cash.exe` from installation folder

***

## Updates and Maintenance

### How do updates work?

ILY Cash uses **automatic update system** (WinSparkle).

**Update checks**:

* 10 seconds after app starts
* Daily at 11:00 AM (Jordan time)

**When update available**:

1. Notification appears
2. Click "Install"
3. App downloads update
4. App restarts automatically
5. New version is active

**No admin privileges required** for updates.

***

### What is the current version?

Check **bottom left** of home screen or About dialog.

**Latest version**: Check [GitHub Releases](https://github.com/psdew2ewqws/ily-cashback/releases)

**Version history**:

* v3.5.9: Auto-update enabled, startup check, Arabic localization, PIN lock bug fix
* v3.5.8: Arabic dialogs, bill validation fix, auto-update framework

***

### Can I disable automatic updates?

**Not recommended** - Updates include:

* Security fixes
* Bug fixes
* New features
* Performance improvements

**No built-in option to disable** - updates are designed to keep all installations current.

***

### Do I need to uninstall before updating?

**No** - Updates install over existing version automatically.

**Manual update process** (if auto-update fails):

1. Download new version from GitHub
2. Close ILY Cash
3. Extract new files over old installation
4. Launch app - new version active

***

## Language and Localization

### How do I change language?

**Not yet implemented** in current version.

**Planned feature**: Language toggle button (English ⟷ Arabic) on home screen.

**Current behavior**: Some dialogs show Arabic messages, especially error dialogs.

Examples:

* "تم استحقاق الكاشباك بنجاح!" (Cashback earned successfully)
* Error messages in Arabic for Arabic-speaking users

***

### Why do some messages appear in Arabic?

**Mixed language interface**: Current version shows:

* UI elements in English
* Some error dialogs in Arabic (for customer communication)
* Success messages in Arabic

**Future versions**: Full language switching will be available.

***

### Arabic text is showing as boxes or ???

**Cause**: Windows missing Arabic font support

**Solution**:

1. Open Windows Settings
2. Go to Time & Language → Language
3. Add Arabic language pack
4. Install Arabic fonts
5. Restart ILY Cash

***

## Technical Questions

### Where is data stored?

**Local storage**: SharedPreferences (Windows registry)

**Data stored**:

* Login token
* Employee information
* Branch details
* Redemption history (fraud detection)
* Transaction cache (duplicate detection)

**No sensitive data**: Passwords not stored locally

***

### Does ILY Cash work offline?

**No** - ILY Cash requires active internet connection for:

* Login authentication
* Transaction processing (backend API calls)
* WhatsApp auto-trigger (WebSocket)
* Fraud detection (blacklist checks)
* Bill auto-submit monitoring

**All features require online connectivity**.

***

### What ports does ILY Cash use?

**HTTPS (443)**:

* Backend API: `cashback-api.meeza.app`
* Auto-update: GitHub releases

**WebSocket**:

* Ably real-time: `wss://realtime.ably.io`

**No firewall configuration needed** for typical networks.

***

### Can I run multiple instances of ILY Cash?

**Not recommended** - Each instance requires separate login session.

**Issues with multiple instances**:

* WebSocket connection conflicts
* Duplicate transaction processing
* File system lock conflicts (bill auto-submit)

**Best practice**: One instance per computer.

***

### Where can I find log files?

**Console logs**: Run from command line to see detailed logs

```cmd
cd C:\Program Files\ILY Cash
ILY_Cash.exe
```

**Look for**:

* 🔒 Security checks
* 💾 Redemption recordings
* 📩 WebSocket messages
* 🚀 Auto-flow events

**No persistent log files** written to disk in current version.

***

## Account and Permissions

### Can I create my own account?

**No** - All accounts are managed by ILY administrators.

**To get an account**: Contact your system administrator or ILY support.

***

### Can I change my password?

**Contact your system administrator** - password changes are managed centrally in the backend system.

**No self-service password reset** in current version.

***

### What happens if I forget my password?

**Contact your system administrator** to reset your password.

**Cannot recover locally** - passwords are not stored in the app.

***

### Can I use the same account on multiple computers?

**Yes** - You can login with the same credentials on different computers.

**But**: Only one active session at a time is recommended.

**WebSocket behavior**: Each login creates a new WebSocket connection - multiple connections may cause conflicts.

***

## Printing (Burn Cashback)

### Do I need a printer for ILY Cash?

**Optional** - Printer only needed for **Burn Cashback** receipts.

**Earn Cashback**: No printer required

**Burn Cashback**: Printer prints redemption receipt for customer.

***

### What type of printer do I need?

**Recommended**: USB thermal printer (POS receipt printer)

**Common models**:

* Epson TM-T20
* Star TSP100
* Any ESC/POS compatible thermal printer

**Configuration**: Printer must be set as default printer in Windows.

***

### Printer is not working - what should I check?

**Basic checks**:

1. Printer powered on
2. USB cable connected
3. Paper loaded
4. Set as default printer in Windows Settings

**Test print**:

```cmd
Control Panel → Devices and Printers → Right-click printer → Print Test Page
```

See [Printing Issues](../troubleshooting/common-errors.md#printing-issues-burn-cashback) for full troubleshooting.

***

## Bill Auto-Submit

### What is bill auto-submit?

A feature that **automatically processes bills** from your POS terminal if not manually submitted within 15 minutes.

**How it works**:

1. POS terminal saves bill to `C:\ily\extracted.txt`
2. ILY Cash monitors this file
3. After 15 minutes, auto-submits to monitoring phone (0790000001)
4. Prevents lost transactions

**Configuration**: Set timer in `assets/config.json`

See [Bill Auto-Submit](../../../#scenario-2-bill-auto-submit) for details.

***

### Can I disable bill auto-submit?

**For administrators**: Modify `assets/config.json`

```json
{
  "auto_submit_timer_minutes": 0
}
```

Setting timer to 0 disables the feature.

**Requires app rebuild** after configuration change.

***

### What is the extracted.txt file format?

**Format**: `{billNumber},{billValue}`

**Example**: `31622831761,25.50`

**Location**: `C:\ily\extracted.txt`

**POS Integration**: Your POS terminal must write to this file in the correct format.

***

## Support and Help

### Where can I get help?

**Documentation**:

* [Getting Started Guide](../../../)
* [Process Cashback How-To](../how-to/process-cashback.md)
* [Troubleshooting Guide](../troubleshooting/common-errors.md)
* [WhatsApp Integration](../how-to/whatsapp-integration.md)

**Support**:

* **Technical issues**: Contact IT support
* **Account problems**: Contact system administrator
* **Feature requests**: Submit on GitHub Issues

**Support hours**: Sunday - Thursday, 9 AM - 5 PM (Jordan Time)

***

### How do I report a bug?

**GitHub Issues**: [Report bug](https://github.com/psdew2ewqws/ily-cashback/issues)

**Include**:

* ILY Cash version (bottom left of home screen)
* Windows version
* Steps to reproduce the bug
* Screenshots of error messages
* Console logs (if available)

***

### How do I request a new feature?

**GitHub Issues**: [Request feature](https://github.com/psdew2ewqws/ily-cashback/issues)

**Include**:

* Clear description of feature
* Use case (why you need it)
* Expected behavior
* Mockups or examples (if applicable)

***

### Is there a user manual?

**This documentation is the user manual**:

1. [**Getting Started**](../../../) - Installation and first steps
2. [**How-To Guides**](../how-to/) - Step-by-step instructions
3. [**Troubleshooting**](../troubleshooting/common-errors.md) - Fix common issues
4. [**FAQ**](general.md) - This file

**For GitBook**: See [GitBook Setup Guide](../../GITBOOK_SETUP_GUIDE.md)

***

## Advanced

### Where is the app configuration file?

**Configuration**: `assets/config.json`

**Settings**:

```json
{
  "monitoring_phone": "0790000001",
  "auto_submit_timer_minutes": 15
}
```

**Requires rebuild**: After modifying config, rebuild app with Flutter.

***

### Where is the security configuration?

**File**: `lib/config/security_config.dart`

**Settings**:

```dart
static const int maxRedemptionsBeforeLock = 1;
static const int redemptionTimeWindowHours = 12;
static const String securityPin = "2941";
```

**Requires rebuild**: Changes require recompiling the app.

***

### How do I enable debug mode?

**Run from command line**:

```cmd
cd C:\Program Files\ILY Cash
ILY_Cash.exe
```

**Console output shows**:

* WebSocket events
* Security checks
* Transaction processing
* Error details
* Auto-trigger activity

**Useful for**: Troubleshooting, monitoring auto-trigger, diagnosing issues

***

### Can I customize the PIN lock threshold?

**Yes** (for administrators):

Edit `lib/config/security_config.dart`:

```dart
// Allow 5 transactions before PIN lock
static const int maxRedemptionsBeforeLock = 5;

// Check transactions within 24 hours (instead of 12)
static const int redemptionTimeWindowHours = 24;
```

**Rebuild app** after changes.

**Warning**: Lower security increases fraud risk.

***

## Related Documentation

* [**Getting Started Guide**](../../../) - Installation and first steps
* [**Process Cashback**](../how-to/process-cashback.md) - Complete transaction guide
* [**WhatsApp Integration**](../how-to/whatsapp-integration.md) - Auto-trigger system
* [**Troubleshooting**](../troubleshooting/common-errors.md) - Fix all errors
* [**Technical Documentation**](../technical/) - Architecture and APIs (coming soon)

***

**Have more questions?** Check the [Troubleshooting Guide](../troubleshooting/common-errors.md) or contact support.
