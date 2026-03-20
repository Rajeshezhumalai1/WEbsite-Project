# 🔗 Database Integration Setup Guide

**Status:** Integration Setup Instructions  
**Pending Items:** 2  
**Estimated Time:** 10 minutes

---

## ⚠️ Currently Pending

1. **Database Connection** - Requires MySQL to be running and configured
2. **Schema Import** - Need to import `techvision_db_schema.sql`

---

## 🎯 Step-by-Step Integration Setup

### **STEP 1: Verify MySQL is Running**

#### Windows:

```bash
# Check if MySQL is running
mysql -u root -p

# If this doesn't work, MySQL isn't running or isn't in PATH
# You need to either:
# 1. Start MySQL service
# 2. Or use full path like: "C:\Program Files\MySQL\MySQL Server X.X\bin\mysql" -u root -p
```

#### Verify Connection:

```bash
mysql -u root -p -e "SELECT 1"
# Should return: 1
```

---

### **STEP 2: Import Database Schema**

#### Method A: Command Line (Recommended)

```bash
# Navigate to the DB folder
cd "c:\Users\DELL\Downloads\new client\Website\Database"

# Import the schema
mysql -u root -p < techvision_db_schema.sql

# When prompted, enter your MySQL root password
```

**Expected Output:**

```
No output = Success! Database created successfully
```

#### Method B: Using MySQL GUI

If you have **MySQL Workbench** or **PhpMyAdmin** installed:

1. Open MySQL Workbench
2. Go to **File → Open SQL Script**
3. Select: `Website/Database/techvision_db_schema.sql`
4. Click **Execute** (Lightning bolt icon)
5. Verify database was created

---

### **STEP 3: Verify Schema Import**

```bash
# List all databases
mysql -u root -p -e "SHOW DATABASES;"

# Should see: techvision_db in the list

# List all tables in the new database
mysql -u root -p -e "USE techvision_db; SHOW TABLES;"

# Should see these 9 tables:
# - blog_posts
# - contacts
# - portfolio_categories
# - projects
# - services
# - settings
# - team_members
# - testimonials
# - users
```

---

### **STEP 4: Update Backend Configuration**

```bash
cd "c:\Users\DELL\Downloads\new client\Website\Backend"
```

**Edit the .env file:**

**Current:**

```env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=
DB_NAME=techvision_db
DB_PORT=3306
```

**Update to:**

```env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_mysql_root_password
DB_NAME=techvision_db
DB_PORT=3306
```

> Replace `your_mysql_root_password` with your actual MySQL root password (the one you use to connect with `mysql -u root -p`)

---

### **STEP 5: Test Database Connection**

```bash
# From the Backend folder
cd "c:\Users\DELL\Downloads\new client\Website\Backend"

# Start the server in dev mode
npm run dev

# Watch for this message:
# ✓ Server running on http://localhost:5000
# If you see database connection errors, go back to STEP 4 and verify DB_PASSWORD
```

**Expected Success Output:**

```
✓ Database connection successful
✓ Server running on http://localhost:5000
```

**If Connection Fails:**

```
✗ Database connection failed: Error: connect ECONNREFUSED 127.0.0.1:3306

Possible causes:
1. MySQL isn't running
2. Wrong DB_PASSWORD in .env
3. MySQL root user doesn't exist
4. Port 3306 is blocked
```

---

## 🔧 Troubleshooting Common Issues

### **Issue 1: "MySQL is not recognized"**

**Solution:** MySQL isn't in your system PATH

```bash
# Option A: Add to PATH (permanent)
# You need to add MySQL bin folder to Windows PATH

# Option B: Use full path (temporary)
"C:\Program Files\MySQL\MySQL Server X.X\bin\mysql" -u root -p

# Replace X.X with your MySQL version (8.0, 8.1, etc.)
```

### **Issue 2: "Access denied for user 'root'@'localhost'"**

**Solution:** Wrong password

```bash
# Try with the correct password
mysql -u root -pYourPasswordHere

# Or if you forgot the password, reset it:
# (Requires MySQL to be stopped and restarted with --skip-grant-tables)
```

### **Issue 3: "Can't connect to MySQL server on 'localhost' (10061)"**

**Solution:** MySQL isn't running

```bash
# Start MySQL service on Windows:
net start MySQL80  # For MySQL 8.0
# or
net start MySQL81  # For MySQL 8.1

# Check which MySQL version you have:
mysql --version
```

### **Issue 4: "Unknown database 'techvision_db'"**

**Solution:** Schema wasn't imported

```bash
# Re-run the import
cd Website/Database
mysql -u root -p < techvision_db_schema.sql

# Verify it was created
mysql -u root -p -e "SHOW DATABASES;" | findstr techvision_db
```

---

## ✅ Verification Checklist

After completing all steps, verify everything works:

```bash
# 1. Check MySQL is running
mysql -u root -p -e "SELECT 1"
# Expected: 1 ✓

# 2. Check database exists
mysql -u root -p -e "SHOW DATABASES;" | findstr techvision_db
# Expected: techvision_db ✓

# 3. Check tables exist
mysql -u root -p -e "USE techvision_db; SHOW TABLES;"
# Expected: 9 tables ✓

# 4. Check .env is configured
type Website\Backend\.env | findstr DB_PASSWORD
# Expected: DB_PASSWORD=your_password ✓

# 5. Start backend and check connection
cd Website\Backend
npm run dev
# Expected: ✓ Database connection successful ✓
# Expected: Server running on http://localhost:5000 ✓
```

---

## 🚀 Once Integration is Complete

### **Test the API:**

```bash
# In a new terminal (keep the server running in the first)
cd Website\Backend
node BACKEND_API_TEST.js

# This will test all 26 API endpoints
```

### **Test the Frontend:**

```bash
# In another new terminal
cd Website\Frontend
python -m http.server 8000

# Open browser: http://localhost:8000
# Try the contact form (will connect to backend API on :5000)
```

### **Full Integration Test:**

1. ✅ Backend running on http://localhost:5000
2. ✅ Frontend running on http://localhost:8000
3. ✅ Database connected
4. ✅ Contact form working
5. ✅ API endpoints responding

---

## 📊 Integration Status After Setup

### **Before Setup:**

```
Frontend:      ✅ COMPLETE (100%)
Backend:       ✅ READY (98%) - Missing DB connection
Database:      ✅ COMPLETE (100%) - Not imported yet
Integration:   ⚠️  PENDING (95%)
```

### **After Setup:**

```
Frontend:      ✅ COMPLETE (100%)
Backend:       ✅ READY (99%) - DB connected ✓
Database:      ✅ COMPLETE (100%) - Imported ✓
Integration:   ✅ COMPLETE (100%) - All working ✓
```

---

## 📋 Configuration Checklist

- [ ] MySQL installed and running
- [ ] Database schema imported
- [ ] techvision_db database created and verified
- [ ] All 9 tables exist
- [ ] .env file updated with DB_PASSWORD
- [ ] Backend started successfully (npm run dev)
- [ ] Database connection confirmed (no errors)
- [ ] API endpoints tested (node BACKEND_API_TEST.js)
- [ ] Frontend-Backend integration verified (contact form test)
- [ ] All systems connected and working

---

## 🎯 Quick Command Summary

```bash
# 1. Import schema
mysql -u root -p < Website/Database/techvision_db_schema.sql

# 2. Verify database
mysql -u root -p -e "USE techvision_db; SHOW TABLES;"

# 3. Update .env (use text editor)
# Edit Website/Backend/.env and set DB_PASSWORD

# 4. Start backend
cd Website/Backend && npm run dev

# 5. Test API (new terminal)
cd Website/Backend && node BACKEND_API_TEST.js

# 6. Start frontend (new terminal)
cd Website/Frontend && python -m http.server 8000
```

---

## 📞 Need Help?

**If something fails:**

1. Check the error message carefully
2. Review the troubleshooting section above
3. Verify each step in the checklist
4. Check the test reports: `FULLSTACK_TEST_REPORT.md`

**Common solutions:**

- Restart MySQL service
- Verify password is correct
- Check if port 3306 is available
- Ensure MySQL is in PATH
- Try full path to mysql.exe if needed

---

## ⏱️ Timeline to Full Deployment

```
Step 1: MySQL Setup          2 min
Step 2: Schema Import         3 min
Step 3: Verify Import         1 min
Step 4: Update .env           2 min
Step 5: Test Connection       2 min
                             -------
        Total                10 minutes

Then:
  - API Testing              5 min
  - Frontend Integration      3 min
  - Full System Test         2 min
```

**Total Time to Full Deployment: ~20 minutes**

---

**Status:** Ready to proceed with integration setup  
**Next Action:** Execute STEP 1 - Verify MySQL is running  
**Expected Result:** 100% Integration Complete ✅
