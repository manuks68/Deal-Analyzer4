# AnuksDeal Mobile App - Storage Management Guide

## 📊 Overview

Your app uses **TWO storage systems** that work together:

### 1. **Local Storage (Browser)**
- Stored in: Browser's `localStorage`
- Location: Your device only
- Persistence: Until you clear browser data
- Keys used: `anuksSavedDeals`, `savedDeals`, `anuksSavedDealsBackup`

### 2. **Supabase Cloud Database**
- Stored in: `saved_deals` table in your Supabase project
- Location: Cloud database
- Persistence: Permanent (until manually deleted)
- Access: Available from any device after sign-in

---

## 🔄 How Syncing Works

### Automatic Sync
The app automatically syncs in these scenarios:
1. **On page load** - If you're signed in, cloud deals auto-download
2. **After sign-in** - Downloads all your cloud deals
3. **When saving a deal** - Saves to both local storage AND cloud (if signed in)

### Manual Sync (NEW!)
You now have buttons to manually control sync (located in **"Admin / Cloud Settings → Advanced Manual Sync"**):

- **↓ Sync from Cloud**: Downloads all deals from Supabase and merges with local deals
  - Cloud data takes priority in conflicts
  - Useful when: You have 13 records in Supabase but don't see them in the app
  
- **↑ Upload to Cloud**: Uploads all local deals to Supabase
  - Useful when: You have local deals that aren't in Supabase yet

- **📊 View Storage Info**: Shows diagnostics comparing local vs cloud counts
  - Located in main "Cloud Status" section for quick access
  - Useful when: You want to verify sync status at a glance

---

## 🛠️ Fix: "My 13 Supabase Records Don't Show Up"

**STEPS TO FIX:**

1. **Check if you're signed in:**
   - Scroll to "Cloud Status" section (10B area)
   - Make sure it says "Cloud connected and signed in"
   - If not, enter your credentials and click "Sign In / Sign Up"

2. **Click "↓ Sync from Cloud" button:**
   - Expand "Admin / Cloud Settings" → "Advanced Manual Sync"
   - Click "↓ Sync from Cloud"
   - This forces a download from Supabase
   - Watch the status message - it should say: "Synced: 13 cloud + X local = Y total deal(s)"

3. **Check Storage Diagnostics:**
   - Click "📊 View Storage Info" button (in main Cloud Status area)
   - This shows you exact counts:
     - Currently Displayed
     - Local Storage count
     - Supabase Cloud count
   
4. **If counts still don't match:**
   - Sign out and sign back in
   - Clear browser cache (⚠️ Warning: This deletes local storage!)
   - After clearing, sign in and click "↓ Sync from Cloud"

---

## 📁 Managing JSON Saved Files

### What are JSON Sync Files?

JSON sync files are **backup files** containing all your saved deals in a portable format.

### When to Use JSON Files:

✅ **Use JSON files when you want to:**
- Create a backup copy of your deals for safekeeping
- Transfer deals to another device without using cloud
- Restore deals after clearing browser data
- Share deals with another user or computer
- Keep an archive of deals at a specific point in time

❌ **You DON'T need JSON files if:**
- You're using Supabase cloud sync (it's automatic)
- You're working on the same device/browser
- You just want to save/load deals normally

### How to Export JSON Files:

1. Go to section **"10B. Saved Deals"**
2. Find **"Manual Backup / Transfer"** panel
3. Click **"Export Sync File"** button
4. A file named `anuksdeal-saveddeals-sync-YYYY-MM-DD.json` downloads
5. Save it somewhere safe (Desktop, cloud storage, etc.)

### How to Import JSON Files:

1. Go to section **"10B. Saved Deals"**
2. Find **"Manual Backup / Transfer"** panel
3. Click **"Import Sync File"** button
4. Select your previously exported JSON file
5. The app will merge those deals into your current saved deals

⚠️ **Important Notes:**
- Import **merges** deals (doesn't replace everything)
- Duplicate deals (same name + submarket) will be skipped or updated
- The import only affects **local storage** - to save to cloud, use "↑ Upload to Cloud" after

---

## 🔍 Storage Priority Rules

When you sync, the app uses these rules to resolve conflicts:

1. **Cloud data wins** if it has a `cloud_row_id` (came from Supabase)
2. **Newer timestamp wins** if both have timestamps
3. **Local data is kept** if cloud doesn't have that specific deal

The merge ensures you don't lose any unique deals from either source.

---

## 🚨 Troubleshooting Common Issues

### Issue 1: "I have 13 records in Supabase but only see 5 in the app"

**Solution:**
```
1. Click "📊 View Storage Info" to confirm counts
2. Expand "Admin / Cloud Settings" → "Advanced Manual Sync"
3. Click "↓ Sync from Cloud" to force download
4. Check "Currently Displayed" should now match Supabase count
```

### Issue 2: "I saved a deal but don't see it in Supabase"

**Possible causes:**
- Not signed in → Sign in first
- Cloud connection failed → Check "Cloud Status" message
- Deal saved locally only → Click "↑ Upload to Cloud"

**Solution:**
```
1. Make sure you're signed in
2. Expand "Admin / Cloud Settings" → "Advanced Manual Sync"
3. Click "↑ Upload to Cloud" to push local deals to Supabase
4. Verify in Supabase dashboard
```

### Issue 3: "My deals disappeared after clearing browser data"

**Solution:**
```
If you have Supabase cloud sync enabled:
1. Sign back in
2. Click "↓ Sync from Cloud"
3. All cloud-saved deals will restore

If you don't have cloud sync:
1. Import your last JSON backup file
2. Consider enabling Supabase cloud sync for the future
```

### Issue 4: "Duplicate deals keep appearing"

**Cause:** Deal key collision (same name + submarket generates same key)

**Solution:**
```
1. Rename one of the duplicate deals
2. Delete the unwanted duplicate
3. Save the deal again
```

---

## 📱 Best Practices

### For Daily Use:
1. ✅ Enable Supabase cloud sync (enter URL + key once)
2. ✅ Sign in when you open the app
3. ✅ Deals auto-sync on save
4. ✅ Occasional JSON backup exports (weekly/monthly)

### For Maximum Safety:
1. ✅ Use Supabase cloud sync (primary backup)
2. ✅ Export JSON file before major work (secondary backup)
3. ✅ Keep old JSON backups in cloud storage (Dropbox/Google Drive)
4. ✅ Test restore process once to ensure backups work

### For Collaboration:
1. ✅ All users connect to same Supabase project
2. ✅ Each user signs in with their own account
3. ✅ Supabase handles data isolation (user_id based)
4. ✅ Use JSON export to share specific deals between users

---

## 🧪 Testing Your Setup

Run this test to verify everything works:

1. **Test Save:**
   - Enter a test deal
   - Click "Save Deal"
   - Check "Saved deals (X)" count increases

2. **Test Cloud Sync:**
   - Click "📊 View Storage Info"
   - Note the counts
   - Click "↓ Sync from Cloud"
   - Verify status message shows correct counts

3. **Test JSON Export:**
   - Click "Export Sync File"
   - Verify file downloads successfully
   - Open file in text editor - should see JSON data

4. **Test JSON Import:**
   - Delete a deal or clear all deals
   - Click "Import Sync File"
   - Select your exported JSON
   - Verify deals restore correctly

---

## 📞 Technical Details

### Storage Keys in localStorage:
- `anuksSavedDeals` - Primary storage key
- `savedDeals` - Backup/legacy key
- `anuksSavedDealsBackup` - Additional backup
- `anuksActiveDealKey` - Tracks currently active deal
- `anuksSavedDealsLastSaved` - Last save timestamp

### Supabase Table Structure:
```
Table: saved_deals
Columns:
  - id (primary key, auto-generated)
  - user_id (UUID, references auth.users)
  - deal_key (text, unique per user)
  - name (text, deal name)
  - submarket (text)
  - data (jsonb, full deal object)
  - created_at (timestamp)
  - updated_at (timestamp)
```

### Deal Key Generation:
The app generates a unique key for each deal using:
```
name + submarket + purchasePrice + units + capRate + pricePerUnit
```
All lowercase, spaces normalized, joined with `|`

This ensures the same deal (with same properties) is recognized as a duplicate.

---

## ✅ Quick Reference

| Action | Button/Location |
|--------|----------------|
| Save current deal | "Save Deal" (section 10A) |
| View all saved deals | "10B. Saved Deals" panel |
| Check sync status | "📊 View Storage Info" (Cloud Status) |
| Download from cloud | "↓ Sync from Cloud" (Advanced Manual Sync) |
| Upload to cloud | "↑ Upload to Cloud" (Advanced Manual Sync) |
| Export JSON backup | "Export Sync File" (Manual Backup) |
| Import JSON backup | "Import Sync File" (Manual Backup) |
| Sign in to cloud | "Admin / Cloud Settings" |

---

## 🎯 Summary

**Your app now has improved storage management with:**

1. ✅ **Manual sync buttons** in "Advanced Manual Sync" to force cloud downloads/uploads
2. ✅ **Storage diagnostics** button in main "Cloud Status" area for quick access
3. ✅ **Better merge logic** that prioritizes cloud data
4. ✅ **Clear status messages** showing what's happening
5. ✅ **JSON backup/restore** for offline transfers
6. ✅ **Removed duplicate buttons** for cleaner UI

**To fix your 13-record issue:**
1. Make sure you're signed in
2. Expand "Admin / Cloud Settings" → "Advanced Manual Sync"
3. Click "↓ Sync from Cloud"
4. Check "📊 View Storage Info" to confirm
5. All 13 records should now appear in "10B. Saved Deals"

---

Need more help? Check the cloud status messages in the app - they provide real-time feedback on what's happening with your storage.
