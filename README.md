## **Pre-Class Work: Analyzing CAP Theorem with MongoDB Atlas M0**

### **Objective:**
Analyze and demonstrate how MongoDB behaves concerning consistency and availability by adjusting read and write concerns within a single-node replica set on MongoDB Atlas's M0 cluster.

### **Tools and Resources:**
- **MongoDB Atlas Account:** [Sign Up Here](https://www.mongodb.com/cloud/atlas/register)
- **MongoDB Shell (mongosh):** Installation commands provided below.
- **Web Browser:** For accessing MongoDB Atlas's admin console.

---

## **Step 1: Set Up a MongoDB Replica Set in Atlas**

### **1.1. Create a MongoDB Atlas Account**

1. **Sign Up:**
   - If you don't have one, go to the [MongoDB Atlas Registration Page](https://www.mongodb.com/cloud/atlas/register) and sign up for a free account.
   - Follow the on-screen instructions to verify your email and complete the registration process.

### **1.2. Create a New Cluster**

1. **Navigate to Clusters:**
   - After logging in, click on the **"Build a Cluster"** button on the Atlas dashboard.

2. **Choose Cluster Tier:**
   - Select the **Free Tier (M0)** option. This tier is sufficient for educational and testing purposes.

3. **Configure Cluster:**
   - **Cloud Provider & Region:**
     - Choose a cloud provider (e.g., **AWS**, **GCP**, **Azure**) and a region close to you to minimize latency.
   - **Cluster Name:**
     - Use the default name or provide a custom name for your cluster.
   - **Additional Settings:**
     - For this exercise, default settings are adequate. You can modify storage options and backup settings if desired, but it's not necessary for basic operations.

4. **Create Cluster:**
   - Click **"Create Cluster"**. Provisioning may take a few minutes. You'll see a notification once your cluster is ready.

### **1.3. Configure Network Access**

1. **Whitelist Your IP Address:**
   - **Navigate to Network Access:**
     - In the left-hand sidebar, under the **"Security"** tab, click on **"Network Access"**.
   - **Add IP Address:**
     - Click **"Add IP Address"**.
     - For simplicity, select **"Allow Access from Anywhere"** by entering `0.0.0.0/0`. **Note:** This is suitable for testing but not recommended for production due to security reasons.
     - Alternatively, enter your specific IP address for restricted access.
   - **Confirm:**
     - Click **"Confirm"** to save the changes.

### **1.4. Create a Database User**

1. **Navigate to Database Access:**
   - Under the **"Security"** tab, click on **"Database Access"**.

2. **Add New User:**
   - Click **"Add New Database User"**.
   - **Authentication Method:**
     - Choose **"Password"**.
   - **Username & Password:**
     - **Username:** Enter a username (e.g., `testUser`).
     - **Password:** Enter a strong password (e.g., `testPass123`) and confirm it.
   - **User Privileges:**
     - Assign the **"Atlas Admin"** role for full access. For more restricted access, select appropriate roles based on your needs.
   - **Add User:**
     - Click **"Add User"** to create the database user.

### **1.5. Connect to Your Cluster Using MongoDB Atlas Data Explorer**

1. **Navigate to Clusters:**
   - In the Atlas dashboard, click on **"Clusters"** in the left-hand sidebar.

2. **Access Data Explorer:**
   - Locate your newly created cluster (e.g., `Cluster0`).
   - Click on **"Collections"** next to your cluster name.
   - This will open the **Data Explorer**.

3. **Create a Database and Collection:**
   - Click **"Add My Own Data"**.
   - **Database Name:** Enter `testdb`.
   - **Collection Name:** Enter `testcollection`.
   - Click **"Create Database"**.
   - Your database and collection are now ready for operations.

---

## **Step 2: Install and Configure MongoDB Shell (mongosh)**

### **2.1. Install mongosh**

The MongoDB Shell (`mongosh`) is a modern, extensible shell for MongoDB. Follow the installation steps based on your operating system.

#### **For macOS:**

1. **Using Homebrew:**
   ```bash
   brew tap mongodb/brew
   brew install mongosh
   ```

2. **Verify Installation:**
   ```bash
   mongosh --version
   ```
   - **Expected Output:**
     ```
     1.x.x
     ```

#### **For Windows:**

1. **Download the Installer:**
   - Visit the [MongoDB Shell Download Page](https://www.mongodb.com/try/download/shell).
   - Select **Windows** as the operating system.
   - Click **Download** to get the `.msi` installer.

2. **Run the Installer:**
   - Locate the downloaded `.msi` file and double-click it to start the installation.
   - Follow the on-screen instructions to complete the installation.

3. **Verify Installation:**
   - Open **Command Prompt** or **PowerShell**.
   - Run:
     ```powershell
     mongosh --version
     ```
   - **Expected Output:**
     ```
     1.x.x
     ```

#### **For Linux:**

1. **Using the Package Manager (Debian/Ubuntu):**
   ```bash
   wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
   echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
   sudo apt-get update
   sudo apt-get install -y mongodb-mongosh
   ```

2. **Verify Installation:**
   ```bash
   mongosh --version
   ```
   - **Expected Output:**
     ```
     1.x.x
     ```

> **Note:** Replace `focal` with your Ubuntu version if different.

---

## **Step 3: Connect to Your MongoDB Atlas Cluster Using mongosh**

### **3.1. Obtain Your Cluster's Connection String**

1. **Navigate to Your Cluster:**
   - In the Atlas dashboard, go to **"Clusters"** and select your cluster (e.g., `Cluster0`).

2. **Connect to Your Cluster:**
   - Click the **"Connect"** button next to your cluster.

3. **Choose Connection Method:**
   - Select **"Connect with MongoDB Shell"**.

4. **Copy the Connection String:**
   - You'll see a connection string similar to:
     ```
     mongosh "mongodb+srv://cluster0.bqvdb.mongodb.net/testdb" --username testUser
     ```
   - **Customize the Connection String:**
     - Replace `cluster0.bqvdb.mongodb.net` with your actual cluster address if different.
     - Replace `testdb` with your database name (`testdb`).
     - Replace `testUser` with your database username (`testUser`).

   - **Example Connection String:**
     ```bash
     mongosh "mongodb+srv://cluster0.bqvdb.mongodb.net/testdb" --username testUser
     ```

### **3.2. Connect Using mongosh**

1. **Open Your Terminal (macOS/Linux) or Command Prompt/PowerShell (Windows).**

2. **Run the Connection Command:**
   - Paste the customized connection string you copied earlier.
   - **Example:**
     ```bash
     mongosh "mongodb+srv://cluster0.bqvdb.mongodb.net/testdb" --username testUser
     ```

3. **Enter Your Password:**
   - When prompted, enter the password you set for `testUser`.
   - **Example Prompt:**
     ```
     Enter password: ***********
     ```

4. **Verify Connection:**
   - Once connected, you should see a prompt similar to:
     ```
     Current Mongosh Log ID:	6723eb4d8ee25819a53da80a
     Connecting to:		mongodb+srv://cluster0.bqvdb.mongodb.net/testdb?appName=mongosh+2.3.3
     Using MongoDB:		7.0.15
     Using Mongosh:		2.3.3

     Atlas atlas-ja136d-shard-0 [primary] testdb>
     ```

---

## **Step 4: Perform Read and Write Operations with Different Consistency Levels**

Even though the **M0 (Free Tier)** cluster operates as a **single-node replica set**, you can still experiment with **write and read concerns** to understand MongoDB's consistency and availability behaviors.

### **4.1. Write Operations with Different Write Concerns**

#### **4.1.1. Insert with Write Concern `w:1` (Default)**

```javascript
db.testcollection.insertOne({ name: "WC1", status: "Write Concern 1" })
```

**Expected Result:**

```json
{
  acknowledged: true,
  insertedId: ObjectId('6723eb9f8ee25819a53da80b')
}
```

- **Explanation:**
  - **`w:1`** ensures that the write is acknowledged after it's written to the primary node.
  - In a single-node replica set (M0), `w:1` is equivalent to `w: "majority"`.

#### **4.1.2. Insert with Write Concern `w: "majority"`**

```javascript
db.testcollection.insertOne(
  { name: "WCMajority", status: "Write Concern Majority" },
  { writeConcern: { w: "majority" } }
)
```

**Expected Result:**

```json
{
  acknowledged: true,
  insertedId: ObjectId('6723eba88ee25819a53da80c')
}
```

- **Explanation:**
  - **`w: "majority"`** ensures that the write is acknowledged by a majority of the replica set members.
  - In a single-node replica set (M0), `w: "majority"` behaves like `w:1` since there's only one node.

#### **4.1.3. Insert with Write Concern `w:0` (Unacknowledged)**

```javascript
db.testcollection.insertOne(
  { name: "WC0", status: "Write Concern 0" },
  { writeConcern: { w: 0 } }
)
```

**Expected Result:**

```json
{
  acknowledged: false,
  insertedId: ObjectId('6723ebb18ee25819a53da80d')
}
```

- **Explanation:**
  - **`w:0`** means the write operation does **not** wait for any acknowledgment from the server.
  - **`acknowledged: false`** indicates that the client did not receive confirmation that the write was successful.
  - **`insertedId`** may still be returned as a local identifier, but it doesn't guarantee that the server processed the write.

> **Note:** On an M0 cluster, which is a single-node replica set, the practical differences between `w:1`, `w: "majority"`, and `w:0` are minimal, but it's essential to understand these concepts for multi-node environments.

### **4.2. Read Operations with Different Read Concerns**

#### **4.2.1. Read with Default Read Concern (`local`)**

```javascript
db.testcollection.find({ name: "WC1" }).pretty()
```

**Expected Result:**

```json
[
  {
    _id: ObjectId('6723eb9f8ee25819a53da80b'),
    name: 'WC1',
    status: 'Write Concern 1'
  }
]
```

- **Explanation:**
  - **`readConcern: "local"`** (default) returns the most recent data available to the node.
  - In a single-node setup, this always reflects the latest writes.

#### **4.2.2. Read with Read Concern `majority`**

```javascript
db.testcollection.find({ name: "WCMajority" }).readConcern("majority").pretty()
```

**Expected Result:**

```json
[
  {
    _id: ObjectId('6723eba88ee25819a53da80c'),
    name: 'WCMajority',
    status: 'Write Concern Majority'
  }
]
```

- **Explanation:**
  - **`readConcern: "majority"`** ensures that the data read has been acknowledged by a majority of the replica set members.
  - On an M0 cluster, `readConcern: "majority"` behaves like `readConcern: "local"`.

#### **4.2.3. Read with Read Concern `linearizable`**

```javascript
db.testcollection.find({ name: "WC1" }).readConcern("linearizable").pretty()
```

**Expected Result:**

```json
[
  {
    _id: ObjectId('6723eb9f8ee25819a53da80b'),
    name: 'WC1',
    status: 'Write Concern 1'
  }
]
```

- **Explanation:**
  - **`readConcern: "linearizable"`** provides the highest level of consistency by ensuring that the read operation reflects all writes that have been acknowledged.
  - In a single-node setup, this behaves similarly to `readConcern: "local"`.

> **Note:** While `linearizable` read concern offers stronger guarantees in multi-node setups, its benefits are limited in a single-node environment.

### **4.3. Verify the Unacknowledged Write (`w:0`)**

To check if the document inserted with `w:0` was successful:

1. **Attempt to Read the Document:**

   ```javascript
   db.testcollection.find({ name: "WC0" }).pretty()
   ```

2. **Expected Outcome:**
   - **If the document appears:**
     - The write was successful, even though it was unacknowledged.
   - **If the document does not appear:**
     - The write may not have been processed, or it failed silently.

> **Important:** Unacknowledged writes (`w:0`) do not guarantee that the write was successful. They are useful for scenarios where performance is prioritized over data reliability, but they should be used cautiously.

---

## **Step 5: Initialize and Verify the Replica Set**

### **5.1. Understanding Replica Set Status on M0**

MongoDB Atlas M0 (Free Tier) operates as a **single-node replica set**. This means:

- **Primary Node:**
  - The only node acting as **PRIMARY**.
  - Handles all read and write operations.

- **Secondary Nodes:**
  - Not applicable in M0 since it's a single-node setup.

### **5.2. Check Replica Set Status**

1. **Run the Following Command in mongosh:**

   ```javascript
   rs.status()
   ```

2. **Expected Output:**

   ```javascript
   {
     "set" : "rs0",
     "date" : ISODate("2023-10-30T12:34:56Z"),
     "myState" : 1,
     "term" : NumberLong(1),
     "heartbeatIntervalMillis" : NumberLong(2000),
     "optimes" : { ... },
     "members" : [
       {
         "_id" : 0,
         "name" : "atlas-ja136d-shard-0.mongodb.net:27017",
         "health" : 1,
         "state" : 1,
         "stateStr" : "PRIMARY",
         "uptime" : 12345,
         "optime" : { ... },
         "optimeDate" : ISODate("2023-10-30T12:34:56Z"),
         "lastHeartbeat" : ISODate("2023-10-30T12:34:56Z"),
         "lastHeartbeatRecv" : ISODate("2023-10-30T12:34:56Z"),
         "pingMs" : NumberLong(15)
       }
     ],
     "ok" : 1
   }
   ```

   - **Key Fields:**
     - **`"stateStr" : "PRIMARY"`** indicates that the node is the primary.
     - **`"myState" : 1`** confirms the node's primary state.

> **Note:** Since M0 is a single-node replica set, the replica set status will always show the node as **PRIMARY**.

---

## **Step 6: Analyze Results and Reflection**

### **6.1. Does MongoDB Accept Writes During the Partition?**

- **Scenario:**  
  On an M0 cluster, network partitions (simulated by isolating nodes) are not applicable since there's only one node. Therefore, MongoDB **always accepts writes** as long as the primary node is operational.

### **6.2. How Does MongoDB Prioritize Consistency Over Availability in This Scenario?**

- **Scenario:**  
  In a single-node replica set, consistency and availability are inherently tied because there's only one source of truth.
