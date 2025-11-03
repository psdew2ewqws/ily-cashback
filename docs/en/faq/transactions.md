# Transaction FAQ

Frequently asked questions about processing cashback transactions (earn and burn).

---

## Earn Cashback Basics

### How do I process an earn cashback transaction?

**Quick steps**:
1. Click **"Earn Cashback"** (green button)
2. Enter customer's 10-digit phone number
3. Customer receives OTP via SMS
4. Fill form: OTP, bill value, bill number, customer name
5. Click **"Submit"**
6. Wait for success dialog

See [Complete Guide](../how-to/process-cashback.md) for detailed walkthrough.

---

### What information do I need from the customer?

**Required information**:
- ✅ Phone number (10 digits, e.g., 0791234567)
- ✅ OTP code (they receive via SMS)
- ✅ Bill value (minimum 2.00 JOD)
- ✅ Bill number (from receipt)
- ✅ Full name

**Optional**:
- Mobile number (usually same as phone)

---

### What if customer doesn't receive OTP?

**Immediate solutions**:
1. **Wait 30-60 seconds** - SMS may be delayed
2. **Check phone number** - Verify 10 digits starting with 07
3. **Check customer's phone** - SMS might be in spam/blocked
4. **Ask customer to check network** - Must have cellular signal

**If still not received**:
- Cancel transaction (go back)
- Re-enter phone number
- System sends new OTP

**Cannot proceed** without valid OTP - it's required for security.

---

### Can I use the same bill number twice?

**No** - Each bill number can only be processed once within **30 days**.

**If duplicate detected**:

![Duplicate Error](../../images/screenshots/error-duplicate-bill.png)

**Error message**: "Bill number already used"

**What to do**:
- Verify bill number is correct
- Check if customer already submitted this bill
- Use different bill number if this is a new purchase

---

### What is the minimum and maximum bill amount?

**Minimum: 2.00 JOD** (enforced)

**Maximum: No limit** (but subject to backend validation)

**Common mistakes**:
- ❌ `1.99` - Too low, will be rejected
- ✅ `2.00` - Minimum acceptable
- ✅ `2.50` - Valid
- ✅ `1500.00` - Valid (large amounts accepted)

---

### Can I process cashback for international customers?

**Jordanian phone numbers only** - Must start with `07`

**Valid formats**:
- ✅ `0791234567`
- ✅ `0781234567`
- ✅ `0770000000`

**Invalid formats**:
- ❌ `+962791234567` (international format)
- ❌ `791234567` (missing 0)
- ❌ `00962791234567` (international prefix)

**System validates** phone format and rejects invalid numbers.

---

### How long does a transaction take?

**Typical timing**:
- Enter phone: 5 seconds
- Wait for OTP: 10-30 seconds
- Fill form: 20-30 seconds
- Submit and process: 3-5 seconds
- **Total: 40-70 seconds** for manual transaction

**Auto-trigger** (WhatsApp): 5-10 seconds total

---

### What if transaction fails after I submit?

**Check error message**:
- Bill too low → Increase amount to ≥ 2.00 JOD
- Duplicate bill → Use different bill number
- Network error → Check internet, retry
- Fraud alert → Customer is blacklisted
- Invalid OTP → Re-enter correct OTP

**Transaction not charged** if error occurs - safe to retry.

See [All Error Scenarios](../troubleshooting/common-errors.md#cashback-processing-errors) for solutions.

---

## OTP Issues

### Customer says OTP is wrong but I entered it correctly?

**Common causes**:

1. **OTP expired** (5-minute limit)
   - Solution: Go back, re-enter phone, get new OTP

2. **Typo in OTP**
   - Solution: Verify with customer character by character
   - Example: `1` vs `l`, `0` vs `O`

3. **Old OTP** (customer reading from previous message)
   - Solution: Ask customer for **most recent SMS**

4. **Backend issue** (rare)
   - Solution: Retry transaction from start

---

### Can I skip OTP entry?

**No** - OTP is mandatory for earn cashback transactions.

**Why required**: Security verification that customer authorized the transaction.

**Burn cashback**: Different flow, OTP may not be required (depends on backend configuration).

---

### OTP field is showing an error before I finish typing?

**Normal behavior** - Field validates as you type.

**If showing red**:
- Finish typing all 4 digits
- Field turns green when valid
- Can then proceed to next field

**If still red after 4 digits**:
- Clear field
- Re-enter OTP
- Verify with customer

---

## Bill Number Issues

### What format should bill number be?

**Any alphanumeric format is accepted**:
- ✅ `12345`
- ✅ `INV-2024-001`
- ✅ `BILL-443`
- ✅ `31622831761`

**Requirements**:
- Must not be empty
- Must be unique (not used in last 30 days)
- Can contain letters, numbers, dashes, underscores

**No length limit** - backend determines valid format.

---

### System says bill number already used - but I haven't processed it before?

**Possible reasons**:

1. **Another employee processed it** at your branch
2. **Customer processed it themselves** via app or website
3. **Duplicate from another store** (same bill number in system)
4. **Auto-trigger processed it** via WhatsApp integration

**Solution**:
- Verify with customer if they already got cashback
- Check transaction history (if available)
- Use different bill number if customer has multiple bills

---

### Can I use partial bill numbers?

**Not recommended** - Use full bill number from receipt.

**Why**:
- Increases risk of duplicates
- Makes transaction tracking difficult
- May violate business policies

**Best practice**: Enter complete bill number exactly as shown on receipt.

---

## Amount and Currency Issues

### Do I enter amount with or without currency?

**Enter number only** - No currency symbol needed

**Examples**:
- ✅ `25.50`
- ❌ `25.50 JOD`
- ❌ `JOD 25.50`
- ❌ `25.50JD`

**System assumes** all amounts are in Jordanian Dinars (JOD).

---

### Should I use period or comma for decimals?

**Use period (.)** - Not comma

**Correct format**:
- ✅ `2.50`
- ✅ `100.00`
- ✅ `15.99`

**Wrong format**:
- ❌ `2,50` (comma - will be rejected)
- ❌ `2.5.0` (multiple periods)
- ❌ `2..50` (double period)

**If error**: Clear field, re-enter with period.

---

### Can I enter amount without decimals?

**Yes** - Both formats accepted:
- ✅ `25` (interpreted as 25.00)
- ✅ `25.00` (explicit decimals)
- ✅ `25.5` (interpreted as 25.50)

**Best practice**: Include decimals for clarity (e.g., `25.00`).

---

### What if bill amount is less than 2 JOD?

**Transaction will be rejected** with error:

![Error Low Amount](../../images/screenshots/error-bill-too-low-arabic.png)

**Solutions**:
1. **Combine multiple small bills** (if allowed by your policy)
2. **Ask customer to make additional purchase** to reach 2.00 JOD
3. **Explain minimum requirement** to customer

**Cannot override** - 2.00 JOD minimum is enforced by backend.

---

## Security and PIN Lock

### Why am I seeing security PIN dialog?

**Trigger**: Same customer phone has processed **1+ transaction within 12 hours**

![Security PIN](../../images/screenshots/security-pin-dialog.png)

**What to do**:
1. Red dialog appears (cannot dismiss)
2. Enter PIN: **2941**
3. Click "Unlock"
4. Transaction proceeds normally
5. Counter resets to 0

**This is fraud prevention** - intentional security feature.

---

### What is the security PIN?

**PIN: 2941**

**Shared with**: All authorized employees at your branch

**When needed**: Customer redemption frequency exceeds limit

**Cannot skip**: Must enter correct PIN to continue transaction

---

### Security PIN appears for every customer - is this normal?

**Not normal** - PIN should only appear for repeat customers within 12 hours.

**Possible issues**:
1. **Testing with same phone number** repeatedly
   - Solution: Use different phone numbers
   - Or wait 12 hours for counter to reset

2. **Security threshold too low** (set to 1 transaction)
   - Solution: Administrator can adjust in `security_config.dart`

3. **System time incorrect**
   - Solution: Verify Windows clock is accurate

**For administrators**: See [Security Configuration](general.md#where-is-the-security-configuration) to adjust thresholds.

---

### Can I bypass PIN lock for trusted customers?

**No manual bypass** - All customers subject to same security rules.

**Exception**: Monitoring phone (0790000001) automatically bypasses security.

**Why strict**: Fraud prevention requires consistent enforcement.

---

### What happens if I enter wrong PIN?

**Transaction blocked** - Customer's cashback will not be processed.

**What to do**:
1. Try again with correct PIN: **2941**
2. If forgotten, contact administrator
3. Transaction can be retried after entering correct PIN

**No lockout** - You can retry PIN entry unlimited times.

---

## Fraud and Blacklist Issues

### Customer is showing orange fraud warning - what does it mean?

![Fraud Alert](../../images/screenshots/fraud-alert-blacklist.png)

**Customer is blacklisted** by backend for suspicious activity.

**What to do**:
- ❌ **DO NOT process transaction**
- Transaction is automatically blocked (cannot override)
- Inform customer they must contact ILY support
- Provide support contact information

**Why blacklisted**: Backend fraud detection flagged this account.

---

### Can I override fraud warnings?

**No** - Fraud warnings (orange dialog) are absolute blocks.

**Only ILY backend** can remove customers from blacklist.

**Do not attempt**:
- Using different phone number for same customer
- Processing with different employee account
- Any workaround to bypass fraud detection

**Consequences**: Processing blacklisted customers violates security policy.

---

### What if customer claims they're not fraudulent?

**Professional response**:

> "I understand your concern. Our system has flagged your account for security review. Please contact ILY customer support at [support number] to resolve this. I'm unable to process transactions while this flag is active."

**Do not**:
- Argue with customer about fraud determination
- Explain specific fraud detection algorithms
- Attempt to process anyway

**Customer must contact ILY support** - only they can resolve blacklist status.

---

## Customer Name Issues

### Does customer name have to match ID?

**No strict validation** - System does not verify name against official ID.

**Best practice**: Enter name as customer provides it.

**Requirements**:
- Must not be empty
- Can contain Arabic or English letters
- Can contain spaces

**If customer refuses name**: Contact administrator for policy guidance.

---

### Can I use nickname or business name?

**Yes** - Any name format is accepted:
- ✅ Full name: `Ahmad Mohammad Al-Said`
- ✅ Nickname: `Abu Mohammad`
- ✅ Business: `Ahmad's Shop`

**System does not enforce** specific name format.

**Your store policy** may have specific requirements.

---

### What if customer name has special characters?

**Accepted characters**:
- Arabic letters: أ ب ت ث...
- English letters: A B C...
- Spaces
- Common punctuation: - ' .

**May cause issues**:
- Emojis
- Special symbols: @ # $ %
- Very long names (>100 characters)

**If error**: Use simplified version of name without special characters.

---

## Network and Connection Issues

### Transaction failed with "Network timeout" - what do I do?

**Immediate steps**:
1. **Check internet**: Open browser, verify connectivity
2. **Retry transaction**: Click submit again
3. **Wait longer**: Network may be slow, not down

**If persists**:
```cmd
ping cashback-api.meeza.app
```

Should show responses. If "Request timed out", network issue confirmed.

**Solution**: Fix internet connection, retry transaction.

See [Network Errors](../troubleshooting/common-errors.md#network-and-connection-errors) for full troubleshooting.

---

### Can I process transactions offline and sync later?

**No** - ILY Cash requires **active internet** for all transactions.

**Why**:
- Backend validates every transaction in real-time
- OTP generation requires backend connection
- Fraud detection checks happen immediately
- No local transaction queue

**Must have connectivity** to process cashback.

---

### What if internet drops during transaction?

**Before submit**: Transaction not sent, safe to retry when reconnected

**During submit**: Transaction may or may not complete
- Check transaction history
- Ask customer if they received confirmation SMS
- If uncertain, contact support before retrying

**After success dialog**: Transaction complete, no issue

**Best practice**: Ensure stable internet before starting busy period.

---

## Auto-Trigger (WhatsApp) Issues

### What if auto-trigger transaction shows error?

**Common auto-trigger errors**:

1. **Bill too low**
   - Backend sent amount < 2.00 JOD
   - Contact backend team to fix

2. **Duplicate bill**
   - Bill already processed
   - Check transaction history

3. **Customer blacklisted**
   - Customer flagged for fraud
   - Cannot process, customer contacts support

4. **PIN lock triggered**
   - Customer exceeded redemption limit
   - Enter PIN 2941 to continue

**Auto-trigger uses same validations** as manual transactions.

See [Auto-Trigger Troubleshooting](../how-to/whatsapp-integration.md#troubleshooting-auto-trigger-issues) for full guide.

---

### Can I cancel auto-triggered transaction?

**No** - Auto-trigger transactions process automatically.

**What happens**:
1. Backend sends transaction
2. App auto-navigates and submits
3. Result appears (success or error)

**Cannot intercept** before submission.

**If error occurs**: Transaction already blocked, no action needed.

---

### Why does auto-trigger ask for PIN?

**Same security rules** apply to auto-triggered transactions.

**If customer has processed 1+ transaction within 12 hours**:
- Security PIN dialog appears
- Must enter 2941 to continue
- Fraud prevention works for all transaction types

**This is intentional** - auto-trigger does not bypass security.

---

## Burn Cashback Issues

### How is burn cashback different from earn?

**Earn Cashback**:
- Customer makes purchase
- You give them cashback points
- Points added to their balance

**Burn Cashback**:
- Customer redeems accumulated points
- You process redemption
- Points deducted from balance
- Customer receives discount/cash

**Different buttons**: Green (earn) vs Red (burn)

---

### What if customer doesn't have enough points to burn?

**System will show error** if redemption amount exceeds available balance.

**What to do**:
- Check customer's credit balance
- Show customer their available points
- Adjust redemption amount
- Or customer can earn more points first

**Cannot burn** more than available balance.

---

### Do I need printer for burn cashback?

**Optional but recommended** - Receipt prints for customer records.

**If no printer**:
- Transaction still processes
- Customer gets confirmation via SMS
- No physical receipt

**Printer required** if your store policy mandates printed receipts.

See [Printer Setup](general.md#what-type-of-printer-do-i-need) for details.

---

## Success and Confirmation

### How do I know transaction was successful?

**Success indicators**:

![Success Dialog](../../images/screenshots/cashback-success.png)

1. **Green success dialog** appears
2. **Transaction confirmation** message
3. **Bill number** displayed
4. **Cashback amount** shown
5. **Customer phone** (masked: ****1234)

**Customer receives**:
- SMS confirmation
- Points added to balance

**Click "OK"** to return to home screen and process next transaction.

---

### Should I give customer a receipt?

**Earn cashback**: No physical receipt needed (SMS confirmation sent)

**Burn cashback**: Receipt recommended
- Prints automatically if printer connected
- Shows redemption details
- Customer proof of transaction

**Your store policy** may have specific requirements.

---

### Can customer verify cashback was added?

**Customer can check balance** via:
- ILY mobile app
- ILY website
- SMS balance inquiry (if available)
- Next transaction at any ILY partner store

**Your store**: No direct way to verify customer balance in ILY Cash app.

---

## Transaction History and Records

### Can I view transaction history?

**No built-in history view** in current version of ILY Cash.

**Backend maintains** all transaction records.

**For records**:
- Contact system administrator
- Request transaction report from backend
- Use backend admin portal (if available)

**Future feature**: Transaction history view planned for future versions.

---

### What if customer disputes a transaction?

**Customer should contact**:
- ILY customer support
- Your store manager
- Provide: Phone number, date, approximate time

**Backend records** include:
- Timestamp
- Employee who processed
- Bill number
- Amount
- Branch location

**You cannot modify** completed transactions - backend administrators handle disputes.

---

### Can I cancel a transaction after success?

**No** - Completed transactions cannot be canceled through ILY Cash app.

**If error occurred**:
- Contact system administrator immediately
- Provide transaction details (bill number, phone, time)
- Administrator may be able to reverse in backend

**Best practice**: Verify all details before clicking submit.

---

## Special Cases

### What about monitoring phone (0790000001)?

**Special testing account**:
- Bypasses security PIN lock
- No redemption limit
- Used for testing and demonstrations

**If you see this number**: It's likely a test transaction or administrator testing.

**Processing**: Same as regular transaction, but no PIN required.

---

### Can I process cashback for employees?

**Depends on store policy** - Technically possible but may be against rules.

**Check with**:
- Store manager
- ILY administrator
- Company policy documents

**System does not block** employee phone numbers automatically.

---

### What if customer has multiple phone numbers?

**Each phone number** = separate account in ILY system.

**Best practice**: Ask customer which number they use for ILY cashback.

**Cannot merge** phone numbers - customer should use same number for all transactions.

---

### Can I process cashback without customer present?

**Not recommended** - OTP verification requires customer's phone.

**Auto-trigger** (WhatsApp integration) allows remote processing with backend approval.

**For manual transactions**: Customer must be present to provide OTP.

---

## Related Documentation

- **[Process Cashback Guide](../how-to/process-cashback.md)** - Complete step-by-step walkthrough
- **[WhatsApp Integration](../how-to/whatsapp-integration.md)** - Auto-trigger system
- **[Troubleshooting](../troubleshooting/common-errors.md)** - Fix all error scenarios
- **[General FAQ](general.md)** - All topics
- **[Security System](../how-to/process-cashback.md#security-features)** - PIN lock and fraud detection

---

**Still have questions?** Check the [General FAQ](general.md) or [Troubleshooting Guide](../troubleshooting/common-errors.md).
