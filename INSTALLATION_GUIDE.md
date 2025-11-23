# Flora E-Commerce - First Time Installation Guide

This guide will walk you through setting up the Flora E-Commerce application from scratch on a fresh Windows machine.

## 📋 Table of Contents

1. [Prerequisites Installation](#prerequisites-installation)
2. [Database Setup](#database-setup)
3. [Backend Setup](#backend-setup)
4. [Frontend Setup](#frontend-setup)
5. [OAuth Configuration (Optional)](#oauth-configuration-optional)
6. [Running the Application](#running-the-application)
7. [Troubleshooting](#troubleshooting)

---

## 1. Prerequisites Installation

### 1.1 Install Java Development Kit (JDK) 17

**Step 1:** Download JDK 17
- Visit [Oracle JDK Downloads](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html) or [Adoptium (OpenJDK)](https://adoptium.net/temurin/releases/?version=17)
- Download the Windows x64 Installer (`.msi` file)

**Step 2:** Install JDK
- Run the downloaded installer
- Follow the installation wizard (default settings are fine)
- Note the installation path (usually `C:\Program Files\Java\jdk-17`)

**Step 3:** Set JAVA_HOME Environment Variable
```powershell
# Open PowerShell as Administrator and run:
[System.Environment]::SetEnvironmentVariable('JAVA_HOME', 'C:\Program Files\Java\jdk-17', 'Machine')
```

Or manually:
- Right-click "This PC" → Properties → Advanced System Settings
- Click "Environment Variables"
- Under "System Variables", click "New"
  - Variable name: `JAVA_HOME`
  - Variable value: `C:\Program Files\Java\jdk-17` (your actual JDK path)
- Find "Path" in System Variables, click "Edit"
- Add new entry: `%JAVA_HOME%\bin`
- Click OK on all dialogs

**Step 4:** Verify Installation
```powershell
# Open a NEW PowerShell window and run:
java -version
# Should show: java version "17.x.x"

javac -version
# Should show: javac 17.x.x
```

---

### 1.2 Install Apache Maven

**Step 1:** Download Maven
- Visit [Maven Downloads](https://maven.apache.org/download.cgi)
- Download the Binary zip archive (e.g., `apache-maven-3.9.x-bin.zip`)

**Step 2:** Extract Maven
- Extract the zip file to `C:\Program Files\Apache\maven`
- Final path should be: `C:\Program Files\Apache\maven\apache-maven-3.9.x`

**Step 3:** Set Maven Environment Variables
```powershell
# Open PowerShell as Administrator and run:
[System.Environment]::SetEnvironmentVariable('MAVEN_HOME', 'C:\Program Files\Apache\maven\apache-maven-3.9.x', 'Machine')
```

Or manually:
- Add System Variable:
  - Variable name: `MAVEN_HOME`
  - Variable value: `C:\Program Files\Apache\maven\apache-maven-3.9.x`
- Edit "Path" variable, add: `%MAVEN_HOME%\bin`

**Step 4:** Verify Installation
```powershell
# Open a NEW PowerShell window and run:
mvn -version
# Should show Maven version and Java version
```

---

### 1.3 Install Node.js and npm

**Step 1:** Download Node.js
- Visit [Node.js Downloads](https://nodejs.org/)
- Download the LTS version (Long Term Support) - Windows Installer (`.msi`)
- Recommended: Version 18.x or higher

**Step 2:** Install Node.js
- Run the installer
- Accept the license agreement
- Use default installation path
- **Important:** Check the box "Automatically install the necessary tools" (includes npm)
- Complete the installation

**Step 3:** Verify Installation
```powershell
# Open a NEW PowerShell window and run:
node --version
# Should show: v18.x.x or higher

npm --version
# Should show: 9.x.x or higher
```

---

### 1.4 Install MySQL 8.0

**Step 1:** Download MySQL
- Visit [MySQL Downloads](https://dev.mysql.com/downloads/installer/)
- Download MySQL Installer for Windows (mysql-installer-community)

**Step 2:** Install MySQL
- Run the installer
- Choose "Developer Default" setup type
- Click "Next" through the installation
- On "Accounts and Roles" page:
  - Set a root password (remember this!)
  - Recommended: `root123` for development (or choose your own)
- Complete the installation

**Step 3:** Verify Installation
```powershell
# Open PowerShell and run:
mysql --version
# Should show: mysql Ver 8.0.x
```

**Step 4:** Test MySQL Connection
```powershell
# Connect to MySQL:
mysql -u root -p
# Enter your root password when prompted

# You should see the MySQL prompt:
mysql>

# Exit MySQL:
exit
```

---

### 1.5 Install Git (Optional but Recommended)

**Step 1:** Download Git
- Visit [Git Downloads](https://git-scm.com/download/win)
- Download the Windows installer

**Step 2:** Install Git
- Run the installer
- Use default settings (recommended)

**Step 3:** Verify Installation
```powershell
git --version
# Should show: git version 2.x.x
```

---

## 2. Database Setup

### 2.1 Create Database

**Step 1:** Open PowerShell and connect to MySQL
```powershell
mysql -u root -p
# Enter your MySQL root password
```

**Step 2:** Create the database
```sql
CREATE DATABASE flora_ecommerce;
exit
```

### 2.2 Run Database Schema

**Step 1:** Navigate to the DB folder
```powershell
cd d:\projects\spingBoot\flora\DB
```

**Step 2:** Import the schema
```powershell
mysql -u root -p flora_ecommerce < 01_schema.sql
# Enter your MySQL root password when prompted
```

### 2.3 Insert Sample Data (Optional)

```powershell
mysql -u root -p flora_ecommerce < 02_sample_data.sql
# Enter your MySQL root password
```

### 2.4 Verify Database Setup

```powershell
mysql -u root -p
```

```sql
USE flora_ecommerce;
SHOW TABLES;
# Should show tables like: users, products, categories, orders, etc.

SELECT * FROM users;
# Should show sample users if you imported sample data

exit
```

---

## 3. Backend Setup

### 3.1 Configure Database Connection

**Step 1:** Navigate to backend resources folder
```powershell
cd d:\projects\spingBoot\flora\backend\src\main\resources
```

**Step 2:** Open `application.properties` in a text editor
```powershell
notepad application.properties
```

**Step 3:** Update MySQL credentials
Find these lines and update with your MySQL credentials:
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/flora_ecommerce
spring.datasource.username=root
spring.datasource.password=root123
```
Replace `root123` with your actual MySQL root password.

**Step 4:** Save and close the file

### 3.2 Build the Backend

**Step 1:** Navigate to backend folder
```powershell
cd d:\projects\spingBoot\flora\backend
```

**Step 2:** Clean and build the project
```powershell
mvn clean install
```

> **Note:** First build will take 5-10 minutes as Maven downloads all dependencies.

**Expected Output:**
```
[INFO] BUILD SUCCESS
[INFO] Total time: XX s
```

### 3.3 Test Backend Startup (Optional)

```powershell
mvn spring-boot:run
```

**Expected Output:**
```
Started FloraEcommerceApplication in X.XXX seconds
```

**Stop the server:** Press `Ctrl + C`

---

## 4. Frontend Setup

### 4.1 Install Dependencies

**Step 1:** Navigate to frontend folder
```powershell
cd d:\projects\spingBoot\flora\frontend
```

**Step 2:** Install npm packages
```powershell
npm install
```

> **Note:** First installation will take 3-5 minutes.

**Expected Output:**
```
added XXX packages in XXs
```

### 4.2 Test Frontend Startup (Optional)

```powershell
npm run dev
```

**Expected Output:**
```
  VITE v5.0.8  ready in XXX ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
```

**Stop the server:** Press `Ctrl + C`

---

## 5. OAuth Configuration (Optional)

> **Note:** OAuth setup is optional. You can skip this section and use traditional email/password authentication.

If you want to enable Google Sign-In, follow the detailed guide:

**See:** [OAUTH_SETUP.md](file:///d:/projects/spingBoot/flora/OAUTH_SETUP.md)

**Quick Steps:**
1. Create Google OAuth credentials at [Google Cloud Console](https://console.cloud.google.com)
2. Copy `application-local.properties.example` to `application-local.properties`
3. Add your Google Client ID and Secret to `application-local.properties`
4. Add Google Client ID to `frontend/src/main.jsx`

---

## 6. Running the Application

### 6.1 Start Backend

**Step 1:** Open PowerShell window #1
```powershell
cd d:\projects\spingBoot\flora\backend
mvn spring-boot:run
```

**Wait for:** `Started FloraEcommerceApplication` message

**Backend URL:** http://localhost:8080

### 6.2 Start Frontend

**Step 2:** Open PowerShell window #2 (keep backend running)
```powershell
cd d:\projects\spingBoot\flora\frontend
npm run dev
```

**Wait for:** `ready in XXX ms` message

**Frontend URL:** http://localhost:5173

### 6.3 Access the Application

**Step 3:** Open your browser and visit:
```
http://localhost:5173
```

### 6.4 Test Login

**Default Admin Credentials:**
- Username: `admin`
- Password: `password123`

**Default User Credentials:**
- Username: `john_doe`
- Password: `password123`

---

## 7. Troubleshooting

### Issue: `java: command not found`

**Solution:**
- Verify JAVA_HOME is set correctly
- Restart PowerShell/Terminal
- Check if `%JAVA_HOME%\bin` is in PATH

```powershell
echo $env:JAVA_HOME
# Should show: C:\Program Files\Java\jdk-17
```

---

### Issue: `mvn: command not found`

**Solution:**
- Verify MAVEN_HOME is set correctly
- Restart PowerShell/Terminal
- Check if `%MAVEN_HOME%\bin` is in PATH

```powershell
echo $env:MAVEN_HOME
# Should show: C:\Program Files\Apache\maven\apache-maven-3.9.x
```

---

### Issue: Port 8080 already in use

**Solution:**
```powershell
# Find process using port 8080
netstat -ano | findstr :8080

# Kill the process (replace XXXX with PID from above)
taskkill /PID XXXX /F
```

Or change the backend port in `application.properties`:
```properties
server.port=8081
```

---

### Issue: MySQL connection refused

**Solution:**
1. Verify MySQL is running:
```powershell
# Open Services (services.msc)
# Find "MySQL80" service
# Start if not running
```

2. Test connection:
```powershell
mysql -u root -p
```

3. Check credentials in `application.properties`

---

### Issue: Frontend won't start - Port 5173 in use

**Solution:**
```powershell
# Find process using port 5173
netstat -ano | findstr :5173

# Kill the process
taskkill /PID XXXX /F
```

---

### Issue: npm install fails

**Solution:**
1. Clear npm cache:
```powershell
npm cache clean --force
```

2. Delete `node_modules` and try again:
```powershell
rm -r node_modules
rm package-lock.json
npm install
```

---

### Issue: Backend build fails - "Client ID must not be empty"

**Solution:**
This happens when OAuth is not configured. You have two options:

**Option 1:** Configure OAuth (see [OAUTH_SETUP.md](file:///d:/projects/spingBoot/flora/OAUTH_SETUP.md))

**Option 2:** Use placeholder values
- Create `application-local.properties` from the example file
- Keep the placeholder values (they won't work for OAuth, but backend will start)

---

## 🎉 Success!

If everything is working:
- ✅ Backend running on http://localhost:8080
- ✅ Frontend running on http://localhost:5173
- ✅ You can login with default credentials
- ✅ You can browse products and use the application

---

## 📚 Next Steps

1. **Explore the Application:**
   - Browse products
   - Add items to cart
   - Place an order
   - Check admin panel

2. **Read Documentation:**
   - [README.md](file:///d:/projects/spingBoot/flora/README.md) - Project overview
   - [DOCUMENTATION.md](file:///d:/projects/spingBoot/flora/DOCUMENTATION.md) - Detailed documentation
   - [OAUTH_SETUP.md](file:///d:/projects/spingBoot/flora/OAUTH_SETUP.md) - OAuth configuration

3. **Development:**
   - Make changes to code
   - Backend auto-reloads with Spring Boot DevTools
   - Frontend auto-reloads with Vite HMR

---

## 🆘 Need Help?

If you encounter issues not covered here:
1. Check the error message carefully
2. Search for the error online
3. Check project documentation
4. Open an issue in the repository

---

**Happy Coding! 🚀**
