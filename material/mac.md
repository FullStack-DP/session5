<!-- 
### Step‑by‑Step Guide to Installing Homebrew on Mac

#### 1. Install Command Line Tools
Before installing Homebrew, make sure Apple’s Command Line Tools are installed:
```bash
xcode-select --install
```
This provides essential compilers and libraries.

#### 2. Run the Homebrew Installation Script
Paste the following command into your Terminal:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
- The script will explain what it does and pause before making changes.  
- It installs Homebrew into `/usr/local` on Intel Macs and `/opt/homebrew` on Apple Silicon (M1/M2/M3/M4).

#### 3. Add Homebrew to Your PATH
The installer usually does this automatically, but if not, add the following line to your shell config file (`~/.zshrc` or `~/.bash_profile`):
```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

#### 4. Verify Installation
Check that Homebrew is installed correctly:
```bash
brew --version
```
You should see the installed version number.

#### 5. Try Installing a Package
For example, install `wget`:
```bash
brew install wget
```
This confirms Homebrew is working.
---


The easiest way to install MongoDB on macOS is with **Homebrew**. Run:  
```bash
brew tap mongodb/brew
brew install mongodb-community
```  
Then start it with:  
```bash
brew services start mongodb-community
```  
This installs MongoDB Community Edition and runs it as a background service.

---

### 🖥️ Step-by-Step Guide to Installing MongoDB on macOS

#### 1. Check Requirements
- macOS **11 (Big Sur) or later** is recommended.  
- 64‑bit processor required.  
- Ensure you have **Homebrew** installed. If not, install it with:  
  ```bash
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```  

#### 2. Install MongoDB via Homebrew
1. **Add MongoDB tap**:  
   ```bash
   brew tap mongodb/brew
   ```
2. **Install MongoDB Community Edition**:  
   ```bash
   brew install mongodb-community
   ```
   This will install the latest stable version (currently MongoDB 8.0).  

#### 3. Start MongoDB
- To run MongoDB as a background service:  
  ```bash
  brew services start mongodb-community
  ```
- To stop it:  
  ```bash
  brew services stop mongodb-community
  ```
- To run manually (without background service):  
  ```bash
  mongod --config /usr/local/etc/mongod.conf
  ```

#### 4. Verify Installation
- Run the MongoDB shell:  
  ```bash
  mongosh
  ```
- If you see a prompt like `test>`, MongoDB is running correctly.  

#### 5. Optional: Install MongoDB Tools
Homebrew also installs **MongoDB Database Tools** (e.g., `mongodump`, `mongorestore`, `mongoimport`, `mongoexport`) automatically since version 4.4.1.
 -->
