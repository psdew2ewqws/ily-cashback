# Quick Screenshot Guide for ILY Cash

Easy manual screenshot capture for documentation.

---

## 🚀 Quick Start

### 1. Run the App

Open ILY Cash:
```
C:\Users\MSI\Desktop\IlyCashbackProject\cashback_app\cashback_app\build\windows\x64\runner\Debug\ILY_Cash.exe
```

### 2. Use Windows Snipping Tool

**Method 1: Keyboard Shortcut (Fastest)**
- Press `Windows + Shift + S`
- Screen will dim
- Drag to select area
- Screenshot copied to clipboard
- Press `Ctrl + V` to paste in Paint or image editor
- Save as PNG

**Method 2: Snipping Tool App**
- Press `Windows` key
- Type "Snipping Tool"
- Click "New"
- Drag to select area
- Click save icon
- Save as PNG

---

## 📸 Screenshots Needed (Checklist)

Save all screenshots to: `docs/images/screenshots/`

### Login & Home (Priority: HIGH)

- [ ] `login-screen.png` - Login screen with empty fields
- [ ] `login-enter-phone.png` - Phone number entered (use 0791234567)
- [ ] `home-screen.png` - Main dashboard after successful login
- [ ] `home-menu.png` - Home showing all menu buttons

### Cashback Flow (Priority: HIGH)

- [ ] `cashback-enter-phone.png` - Cashback form with phone filled
- [ ] `cashback-enter-bill.png` - Bill number filled (e.g., "Bill123")
- [ ] `cashback-enter-amount.png` - Amount filled (e.g., "25.50")
- [ ] `cashback-review-submit.png` - All fields filled, ready to submit
- [ ] `cashback-success.png` - ✅ **ARABIC** success dialog

### Error Dialogs (Priority: HIGH - NEED ARABIC)

- [ ] `error-bill-too-low.png` - Error for bill < 2 JOD (**ARABIC** dialog)
- [ ] `fraud-alert.png` - RED blacklist warning (**ARABIC** dialog)
- [ ] `error-customer-not-found.png` - Customer not registered error

### Other Screenshots (Priority: MEDIUM)

- [ ] `error-connection.png` - Network connection error
- [ ] `whatsapp-auto-success.png` - Auto-success from WhatsApp (**ARABIC**)
- [ ] `server-status-indicator.png` - Zoom in on server status (top of app)

### System Screenshots (Priority: LOW)

- [ ] `system-tray-ily.png` - Full screen showing ILY icon in Windows tray
- [ ] `system-tray-menu.png` - Right-click menu from tray icon

---

## 🎯 How to Trigger Each Screenshot

### Login Screenshots

**login-screen.png:**
1. Launch app
2. Wait for login screen to appear
3. Don't type anything
4. Take screenshot

**login-enter-phone.png:**
1. On login screen
2. Type phone: `0791234567`
3. Don't click Login yet
4. Take screenshot

**home-screen.png:**
1. Login with valid credentials
2. Wait for home dashboard
3. Take screenshot

---

### Cashback Screenshots

**cashback-enter-phone.png:**
1. Click "Earn Cashback" from home
2. Type phone: `0791234567`
3. Leave other fields empty
4. Take screenshot

**cashback-enter-bill.png:**
1. Continue from above
2. Type bill number: `Bill123`
3. Leave amount empty
4. Take screenshot

**cashback-enter-amount.png:**
1. Continue from above
2. Type amount: `25.50`
3. Don't click Submit yet
4. Take screenshot

**cashback-review-submit.png:**
1. All fields filled:
   - Phone: 0791234567
   - Bill: Bill123
   - Amount: 25.50
2. Don't click Submit yet
3. Take screenshot

**cashback-success.png:**
1. Fill form with VALID customer data
2. Click Submit
3. Wait for success dialog (ARABIC)
4. Take screenshot quickly (auto-closes in 10 seconds!)

---

### Error Dialog Screenshots

**error-bill-too-low.png:**
1. Open Earn Cashback
2. Fill in:
   - Phone: 0791234567
   - Bill: Test123
   - Amount: `1.50` (below 2 JOD minimum!)
3. Click Submit
4. Error dialog appears in ARABIC
5. Take screenshot

**fraud-alert.png:**
1. Need a BLACKLISTED phone number
2. Fill cashback form with blacklisted number
3. Click Submit
4. RED fraud warning appears (ARABIC)
5. Take screenshot

**error-customer-not-found.png:**
1. Fill cashback form
2. Use fake phone: `0799999999`
3. Bill: Test123
4. Amount: 25.50
5. Click Submit
6. Error appears
7. Take screenshot

---

## 💡 Tips for Good Screenshots

### 1. Clean Background
- Close other windows
- Clean desktop
- Hide personal info

### 2. Full Window Capture
- Capture entire app window
- Include title bar
- Include all UI elements

### 3. Clear Text
- Use high DPI monitor if available
- Don't zoom in browser
- Keep default font sizes

### 4. Consistent Size
- All screenshots should be similar dimensions
- Crop consistently
- Maintain aspect ratio

### 5. Arabic Dialogs
- Make sure Arabic text is clear
- Capture entire dialog
- Don't cut off text

---

## 🔄 After Taking Screenshots

### 1. Review Each Screenshot
- Check if text is readable
- Ensure nothing is cut off
- Verify no personal data visible

### 2. Rename if Needed
- Use exact names from checklist
- All lowercase
- Use hyphens (not spaces)

### 3. Move to Documentation Folder
Move all screenshots to:
```
C:\Users\MSI\Desktop\IlyCashbackProject\cashback_app\cashback_app\docs\images\screenshots\
```

### 4. Compress (Optional)
- Use TinyPNG: https://tinypng.com
- Or Windows built-in compression
- Target: < 200KB per screenshot

---

## 🎨 Adding Annotations (Optional)

### Tools to Use

**Free Tools:**
- **ShareX** (https://getsharex.com) - Best, free, powerful
- **Greenshot** (https://getgreenshot.org) - Simple, easy
- **Paint** (Built-in Windows) - Basic but works

### What to Add

**Red Boxes:** Around buttons, fields, important UI
**Arrows:** Pointing to where to click
**Numbers:** For step sequence (1, 2, 3...)
**Text Labels:** Brief explanations

### Example

Before:
![Raw screenshot](examples/raw-screenshot.png)

After:
![Annotated screenshot](examples/annotated-screenshot.png)
- Red box around "Submit" button
- Arrow pointing to phone field
- Number "1" on first field

---

## 📋 Quick Checklist

Before closing this guide:

- [ ] ILY Cash app is running
- [ ] Know how to use Windows Snipping Tool (Win + Shift + S)
- [ ] Have list of screenshots needed
- [ ] Know how to trigger each screen
- [ ] Created screenshots folder if needed
- [ ] Ready to start capturing!

---

## 🆘 Troubleshooting

**Can't find Snipping Tool?**
- Press `Windows` key
- Type "Snipping Tool"
- It's built into Windows 10/11

**Screenshot too blurry?**
- Use higher resolution monitor
- Don't use Windows scaling
- Capture at 100% zoom

**Can't trigger error dialogs?**
- You may need access to backend to create test scenarios
- Ask development team for test data
- Use production-like test environment

**App not opening?**
- Check path is correct
- Try Debug build instead of Release
- Rebuild app: `flutter build windows`

---

## ✅ Quick Command Reference

**Run App:**
```bash
C:\Users\MSI\Desktop\IlyCashbackProject\cashback_app\cashback_app\build\windows\x64\runner\Debug\ILY_Cash.exe
```

**Screenshot Shortcut:**
```
Windows + Shift + S
```

**Save Location:**
```
C:\Users\MSI\Desktop\IlyCashbackProject\cashback_app\cashback_app\docs\images\screenshots\
```

---

## 🎯 Priority Order

**Do First (Essential):**
1. login-screen.png
2. home-screen.png
3. cashback-success.png (ARABIC)
4. error-bill-too-low.png (ARABIC)
5. fraud-alert.png (ARABIC RED dialog)

**Do Second (Important):**
6. All cashback flow screenshots
7. Other error dialogs
8. WhatsApp screenshots

**Do Last (Nice to have):**
9. System tray screenshots
10. Server status indicators

---

**Ready to start? Open the app and press `Windows + Shift + S` to begin!**
