# GitBook Setup Guide for ILY Cash Documentation

Complete guide to uploading and managing your ILY Cash documentation on GitBook.

---

## 🎯 Quick Overview

**What is GitBook?**
- Modern documentation platform with beautiful UI
- Supports Markdown files
- Real-time collaboration
- Free plan available for public docs
- Can sync with GitHub automatically

**What You'll Upload:**
- All documentation from `docs/` folder
- 9 screenshots mapped in SCREENSHOT_MAPPING.md
- User guides, API docs, troubleshooting

---

## 📋 Prerequisites

Before starting, ensure you have:
- [ ] GitHub account (you already have one)
- [ ] All documentation files in `docs/` folder
- [ ] 9 screenshots ready to upload
- [ ] Email address for GitBook account

---

## 🚀 Step 1: Create GitBook Account

### Option A: Sign Up with GitHub (Recommended)

1. **Go to GitBook**: https://www.gitbook.com
2. **Click "Sign Up"** (top right)
3. **Choose "Continue with GitHub"**
4. **Authorize GitBook** to access your GitHub account
5. **Complete profile** (name, workspace name)

### Option B: Sign Up with Email

1. **Go to GitBook**: https://www.gitbook.com
2. **Click "Sign Up"**
3. **Enter email and password**
4. **Verify email** (check inbox)
5. **Complete profile setup**

---

## 📚 Step 2: Create Your Documentation Space

1. **After login**, click **"New Space"** button
2. **Choose template**: Select "Blank" or "Product Documentation"
3. **Name your space**: "ILY Cash Documentation" or "ILY Cashback System"
4. **Set visibility**:
   - **Public**: Anyone can read (recommended for open source)
   - **Private**: Only invited members (requires paid plan)
5. **Click "Create Space"**

---

## 🔗 Step 3: Connect to GitHub (Recommended Method)

**Why GitHub Integration?**
- Auto-sync documentation changes
- Keep docs in version control
- Edit locally, publish automatically
- Team collaboration easier

### Setup GitHub Sync

1. **In your GitBook space**, click **Settings** (gear icon, left sidebar)
2. **Go to "Integrations"** tab
3. **Click "GitHub"** → **"Install GitHub App"**
4. **Choose your repository**: `cashback_app` or create new `ily-cash-docs` repo
5. **Select sync options**:
   - **Branch**: `main` or `docs`
   - **Direction**: GitHub → GitBook (one-way) or Bidirectional
   - **Root directory**: `/docs/` (only sync docs folder)
6. **Click "Save"**

### Prepare Your GitHub Repo

```bash
# Create a dedicated docs branch (optional but recommended)
git checkout -b docs

# Push your docs to GitHub
git add docs/
git commit -m "Add ILY Cash documentation for GitBook"
git push origin docs
```

After push, GitBook will automatically import your docs!

---

## 📤 Step 4: Manual Upload (Alternative Method)

If you prefer not to use GitHub integration:

### Upload Individual Files

1. **In GitBook editor**, click **"+ New Page"** (left sidebar)
2. **Name the page** (e.g., "Getting Started")
3. **Copy content** from your `.md` files
4. **Paste into GitBook editor**
5. **Repeat** for each documentation file

### Folder Structure to Recreate

```
ILY Cash Documentation/
├─ Getting Started/
│  ├─ Installation
│  ├─ First Login
│  └─ Quick Start Guide
│
├─ How-To Guides/
│  ├─ Process Cashback
│  ├─ Handle Errors
│  └─ Daily Operations
│
├─ Troubleshooting/
│  ├─ Common Errors
│  ├─ Connection Issues
│  └─ Security Problems
│
├─ Technical Docs/
│  ├─ PIN Lock Bug Fix
│  ├─ Security Config
│  └─ Auto-Update System
│
└─ Screenshots/
   └─ Screenshot Mapping
```

---

## 🖼️ Step 5: Upload Screenshots

### Method 1: Drag & Drop (Easiest)

1. **Open any page** in GitBook editor
2. **Drag image file** from your file explorer
3. **Drop onto the page** where you want it
4. **GitBook uploads** and inserts image automatically
5. **Add caption** (optional): Click image → "Add caption"

### Method 2: Upload Button

1. **Type `/image`** in the editor
2. **Click "Upload"** button
3. **Select screenshot** from file browser
4. **Click "Open"**
5. **Image inserted** at cursor position

### Method 3: GitHub (Auto-Upload)

If using GitHub integration:

1. **Create screenshots folder** in your repo:
   ```bash
   mkdir -p docs/.gitbook/assets
   cp docs/images/screenshots/* docs/.gitbook/assets/
   ```

2. **Reference in Markdown**:
   ```markdown
   ![Login Screen](.gitbook/assets/login-screen.png)
   ```

3. **Commit and push**:
   ```bash
   git add docs/.gitbook/assets/
   git commit -m "Add screenshots for documentation"
   git push origin docs
   ```

4. **GitBook auto-syncs** and displays images

---

## 📝 Step 6: Organize Your Documentation

### Create Page Groups (Folders)

1. **Click "+" next to "Pages"** (left sidebar)
2. **Choose "Group"**
3. **Name the group** (e.g., "How-To Guides")
4. **Drag pages** into the group
5. **Nest groups** by dragging one into another

### Set a Home Page

1. **Create "Welcome" page**
2. **Click "..." next to page name**
3. **Select "Set as home page"**
4. **This page shows** when users visit your docs

### Recommended Home Page Content

```markdown
# ILY Cash Documentation

Welcome to the ILY Cash cashback management system documentation.

## What is ILY Cash?

ILY Cash is a Windows desktop application for managing cashback transactions...

## Quick Links

- 🚀 [Getting Started](getting-started/installation.md)
- 📖 [Process Cashback](how-to/process-cashback.md)
- 🔧 [Troubleshooting](troubleshooting/common-errors.md)
- 🔐 [Security Features](technical/security-config.md)

## Recent Updates

- **v3.5.10**: Fixed critical PIN lock security bug
- **v3.5.9**: Added Arabic localization
- **v3.5.8**: Bill validation improvements
```

---

## 🎨 Step 7: Customize Your GitBook

### Theme & Appearance

1. **Go to Settings** → **Appearance**
2. **Choose color theme**: Light, Dark, or Auto
3. **Set primary color**: Brand color for links and buttons
4. **Upload logo**: Your ILY Cash logo (optional)

### Navigation Settings

1. **Settings** → **Navigation**
2. **Enable/disable**:
   - Table of Contents (left sidebar)
   - Page outline (right sidebar)
   - Search bar
   - Breadcrumbs

### Domain Settings (Optional)

1. **Settings** → **Domain**
2. **Choose subdomain**: `ily-cash.gitbook.io` (free)
3. **Or add custom domain**: `docs.ilycash.com` (paid plan)

---

## 🔍 Step 8: Add Search & Navigation

### Enable Search

Search is **enabled by default** in GitBook. It searches:
- All page content
- Headings
- Code blocks
- Image captions

### Add Table of Contents

GitBook automatically generates TOC from your page structure.

**To customize**:
1. **Reorder pages** by dragging in sidebar
2. **Nest pages** to create hierarchy
3. **Hide pages** by clicking "..." → "Hide from navigation"

### Add Quick Links

Create a "Links" page with important external resources:

```markdown
# Quick Links

## External Resources

- [GitHub Repository](https://github.com/yourusername/cashback_app)
- [Report an Issue](https://github.com/yourusername/cashback_app/issues)
- [Download Latest Release](https://github.com/yourusername/cashback_app/releases)

## Internal Tools

- [Auto-Update Server](http://your-server.com/appcast.xml)
- [Admin Dashboard](http://your-admin-url.com)
```

---

## 📊 Step 9: Add Your Specific Documentation

### Pages to Create (Based on Your Docs)

#### 1. Welcome Page
```markdown
# ILY Cash Documentation

[Content from docs/en/README.md or create introduction]
```

#### 2. Getting Started Section
- **Installation**: How to install ILY Cash
- **First Login**: Using the login screen (use `login-screen.png`)
- **Home Screen**: Main dashboard overview (use `home-screen.png`)

#### 3. How-To Guides Section
- **Process Cashback**: Complete guide (use screenshots 4, 1, 8)
  - Enter phone (`cashback-enter-phone.png`)
  - Review & submit (`cashback-review-submit.png`)
  - Success (`cashback-success.png`)
- **Handle Errors**: Common errors and solutions

#### 4. Troubleshooting Section
- **Common Errors**: All error types
  - Bill too low (`error-bill-too-low-arabic.png`)
  - Duplicate bill (`error-duplicate-bill.png`)
  - Customer blocked (`fraud-alert-blacklist.png`)
- **Security Features**: PIN lock system (`security-pin-dialog.png`)

#### 5. Technical Documentation Section
- **PIN Lock Bug Fix**: From `docs/PIN_LOCK_BUG_FIX.md`
- **Security Configuration**: From `docs/PIN_LOCK_DEBUG_TEST.md`
- **Auto-Update System**: How updates work

---

## 🔐 Step 10: Security & Privacy Settings

### Access Control (Paid Plans)

If you upgrade to paid plan:
1. **Settings** → **Access Control**
2. **Invite team members** by email
3. **Set permissions**: Admin, Editor, Reader
4. **Require authentication** for private docs

### Public Sharing (Free Plan)

Your docs are public by default:
- **Share link**: `https://your-space.gitbook.io`
- **Embed pages**: Copy embed code for website
- **Export PDF**: Settings → Export

---

## 🚀 Step 11: Publish Your Documentation

### Publish Changes

1. **Make edits** in GitBook editor
2. **Click "Merge"** (top right)
3. **Add commit message**: "Add initial documentation"
4. **Click "Merge in [branch name]"**
5. **Changes go live** immediately

### Share Your Docs

Your documentation is now live at:
```
https://your-space-name.gitbook.io
```

Share this link:
- In your app's About dialog
- On GitHub README
- With your users
- On social media

---

## 📱 Step 12: GitBook Mobile & Desktop Apps

### GitBook Editor App (Optional)

Download for better editing experience:
- **Windows/Mac**: https://www.gitbook.com/downloads
- **Features**: Offline editing, faster performance, native feel

### GitBook Mobile App

View docs on mobile:
- **iOS**: App Store → "GitBook"
- **Android**: Play Store → "GitBook"

---

## 🔄 Step 13: Maintain Your Docs

### Update Documentation

**With GitHub Integration**:
```bash
# Edit docs locally
nano docs/how-to/process-cashback.md

# Commit changes
git add docs/
git commit -m "Update cashback process guide"
git push origin docs

# GitBook auto-syncs within 1-2 minutes
```

**With Manual Updates**:
1. Open GitBook editor
2. Edit page directly
3. Click "Merge" to publish

### Version Control

GitBook keeps history of all changes:
1. **Settings** → **History**
2. **View all revisions**
3. **Revert to any version** if needed
4. **Compare versions** side-by-side

---

## 📋 Your Upload Checklist

Use this checklist to track your progress:

### Documentation Files
- [ ] Welcome/Home page created
- [ ] Getting Started section uploaded
- [ ] How-To Guides section uploaded
- [ ] Troubleshooting section uploaded
- [ ] Technical docs uploaded (PIN_LOCK_BUG_FIX.md, etc.)

### Screenshots (9 total)
- [ ] `home-screen.png` uploaded
- [ ] `login-screen.png` uploaded
- [ ] `cashback-enter-phone.png` uploaded
- [ ] `cashback-review-submit.png` uploaded
- [ ] `cashback-success.png` uploaded
- [ ] `error-bill-too-low-arabic.png` uploaded
- [ ] `error-duplicate-bill.png` uploaded
- [ ] `fraud-alert-blacklist.png` uploaded
- [ ] `security-pin-dialog.png` uploaded

### Organization
- [ ] Page groups created
- [ ] Pages nested properly
- [ ] Home page set
- [ ] Navigation order correct

### Configuration
- [ ] Space name set
- [ ] Theme/colors customized
- [ ] Logo uploaded (optional)
- [ ] Domain configured (optional)

### Publishing
- [ ] All changes merged
- [ ] Documentation live
- [ ] Share link tested
- [ ] Shared with users

---

## 🎯 Quick Start Commands (GitHub Method)

If you want to use GitHub integration, run these commands:

```bash
# 1. Create docs branch
git checkout -b docs

# 2. Organize screenshots for GitBook
mkdir -p docs/.gitbook/assets
cp docs/images/screenshots/*.png docs/.gitbook/assets/

# 3. Update image references in markdown files
# (GitBook will do this automatically if you upload via editor)

# 4. Commit everything
git add docs/
git commit -m "Prepare documentation for GitBook"

# 5. Push to GitHub
git push origin docs

# 6. Connect GitBook to this branch
# (Follow Step 3 in this guide)
```

---

## 💡 Pro Tips

### 1. Use GitBook Features

**Hints/Callouts**:
```markdown
{% hint style="info" %}
This is an informational message
{% endhint %}

{% hint style="warning" %}
This is a warning message
{% endhint %}

{% hint style="danger" %}
This is a critical warning
{% endhint %}
```

**Tabs**:
```markdown
{% tabs %}
{% tab title="Windows" %}
Instructions for Windows...
{% endtab %}

{% tab title="Mac" %}
Instructions for Mac...
{% endtab %}
{% endtabs %}
```

**Code Blocks**:
```markdown
```dart
// Your code here with syntax highlighting
void main() {
  print('Hello World');
}
```
```

### 2. SEO Optimization

1. **Add descriptions**: Settings → SEO → Meta descriptions
2. **Use keywords**: In headings and content
3. **Add alt text**: To all images
4. **Internal linking**: Link between related pages

### 3. Analytics (Optional)

1. **Settings** → **Integrations**
2. **Add Google Analytics** tracking code
3. **Monitor page views**, popular pages, user flow

### 4. Multi-Language Support

If you want Arabic and English docs:
1. **Settings** → **Languages**
2. **Add Arabic** as second language
3. **Create variants** of each page
4. **Language switcher** appears automatically

---

## 🆘 Troubleshooting

### Images Not Showing

**Problem**: Images show broken link icon

**Solution**:
- Check file path is correct
- Ensure image uploaded to GitBook
- Use relative paths: `./assets/image.png`
- Verify image format is PNG/JPG/GIF

### GitHub Sync Not Working

**Problem**: Changes on GitHub not appearing in GitBook

**Solution**:
- Check GitBook → Settings → Integrations → GitHub
- Verify webhook is active
- Re-authorize GitHub connection
- Check branch name is correct
- Wait 2-3 minutes for sync

### Can't Upload Large Files

**Problem**: Image upload fails

**Solution**:
- Compress images (max 10MB per file)
- Use PNG format for screenshots
- Reduce image dimensions if very large
- Split large PDFs into separate files

---

## 📞 Support Resources

### GitBook Help

- **Documentation**: https://docs.gitbook.com
- **Support**: support@gitbook.com
- **Community**: https://github.com/GitbookIO/community
- **Status**: https://status.gitbook.com

### Your Next Steps

1. **Create GitBook account**: https://www.gitbook.com
2. **Create new space**: "ILY Cash Documentation"
3. **Choose integration method**: GitHub or Manual
4. **Upload documentation files**
5. **Add your 9 screenshots**
6. **Organize pages** into sections
7. **Publish and share** with users

---

## ✅ Completion

Once you've completed this guide:
- Your documentation is **live and accessible**
- Users can **search and navigate** easily
- Updates are **simple** (edit locally → push → auto-sync)
- Professional appearance with **GitBook's UI**

**Your docs will be at**: `https://your-space.gitbook.io`

Share this with your users and enjoy your beautiful documentation! 🎉

---

**Need help?** Refer to this guide or check GitBook's official documentation.

**Found an issue?** Update this guide and keep it current.
