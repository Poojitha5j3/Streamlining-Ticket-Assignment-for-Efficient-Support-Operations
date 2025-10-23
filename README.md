# ** Streamlining Ticket Assignment for Efficient Support Operations**

### **Objective**

Implement an automated ticket routing system in **ServiceNow** for **ABC Corporation** to:

* Improve operational efficiency
* Ensure accurate ticket assignment
* Reduce resolution delays
* Enhance customer satisfaction
* Optimize support team resource utilization

---

## **Implementation Steps**

### **1. User Creation**

**Path:** *ServiceNow → All → Users (System Security) → New*

* Enter user details → **Submit**
* Repeat as required to create additional users

**Example Users:**

* Katherine Pierce
* Manne Niranjan

---

### **2. Group Creation**

**Path:** *ServiceNow → All → Groups (System Security) → New*

* Enter group details → **Submit**
* Repeat for additional groups

**Example Groups:**

* Certificates Group
* Platform Group

---

### **3. Role Creation**

**Path:** *ServiceNow → All → Roles (System Security) → New*

* Enter role details → **Submit**
* Repeat as needed

**Example Roles:**

* Certification_Role
* Platform_Role

---

### **4. Table Creation**

**Path:** *ServiceNow → All → Tables (System Definition) → New*

| Field         | Value                                           |
| ------------- | ----------------------------------------------- |
| **Label**     | Operations Related                              |
| **Menu Name** | Operations Related                              |
| **Options**   | Enable “Create module” & “Create mobile module” |

**Add Columns:**

* *Issue* (Choice field)
* *Assigned to Group*
* *Other custom columns as needed*

**Choices for the Issue Field:**

* Unable to login to platform
* 404 Error
* Regarding Certificates
* Regarding User Expired

---

### **5. Assign Roles & Users to Groups**

**Example Assignments:**

| Group              | User             | Role               |
| ------------------ | ---------------- | ------------------ |
| Certificates Group | Katherine Pierce | Certification_Role |
| Platform Group     | Manne Niranjan   | Platform_Role      |

**Path:** *All → Groups → [Select Group] → Add User → Add Role → Save*

---

### **6. Assign Roles to Table**

**Path:** *All → Tables → Operations Related → Application Access*

* **Read/Write Access:** Require both `Platform_Role` and `Certification_Role`

---

### **7. Create ACLs (Access Controls)**

**Path:** *All → Access Control (System Security) → New*

* Define ACLs for table fields
* Required Role: **Admin**
* Create separate ACLs for:

  * Read Access
  * Write Access
  * Field-Level Restrictions

Repeat for four key fields as needed.

---

### **8. Flow Designer – Automated Ticket Assignment**

#### **Flow 1: Regarding Certificates**

**Path:** *Flow Designer → New Flow*

| Setting         | Value                                                  |
| --------------- | ------------------------------------------------------ |
| **Name**        | Regarding Certificate                                  |
| **Application** | Global                                                 |
| **Run As**      | System User                                            |
| **Trigger**     | Record Created/Updated → Table: Operations Related     |
| **Condition**   | Issue = “Regarding Certificates”                       |
| **Action**      | Update Record → Assigned to Group = Certificates Group |

→ **Save & Activate**

---

#### **Flow 2: Regarding Platform**

**Path:** *Flow Designer → New Flow*

| Setting         | Value                                                                            |
| --------------- | -------------------------------------------------------------------------------- |
| **Name**        | Regarding Platform                                                               |
| **Application** | Global                                                                           |
| **Run As**      | System User                                                                      |
| **Trigger**     | Record Created/Updated → Table: Operations Related                               |
| **Condition**   | Issue = “Unable to login to platform” OR “404 Error” OR “Regarding User Expired” |
| **Action**      | Update Record → Assigned to Group = Platform Group                               |

→ **Save & Activate**

---

## **📂 Project Structure**

```
ABC-Ticket-Routing
│
├── Users/
│   ├── Katherine Pierce (Certificates Group)
│   └── Manne Niranjan (Platform Group)
│
├── Groups/
│   ├── Certificates Group
│   └── Platform Group
│
├── Roles/
│   ├── Certification_Role
│   └── Platform_Role
│
├── Tables/
│   └── u_operations_related
│       ├── Issue (Choice field)
│       ├── Assigned to Group
│       └── Other custom columns
│
├── ACLs/
│   ├── Read Access
│   ├── Write Access
│   └── Field-Level Restrictions
│
└── Flows/
    ├── Regarding Certificate → Assigns to Certificates Group
    └── Regarding Platform → Assigns to Platform Group
```

---

## **Outcome**

The automated ticket routing system in **ServiceNow** successfully:

* Streamlines support operations
* Ensures accurate and intelligent ticket assignment
* Minimizes manual intervention and resolution delays
* Optimizes utilization of support resources
* Improves service quality and overall customer satisfaction
