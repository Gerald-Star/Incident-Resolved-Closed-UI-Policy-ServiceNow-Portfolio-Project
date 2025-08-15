# Incident Resolved/Closed UI Policy with Client-Side API – ServiceNow Portfolio Project.


## Project Overview
This project demonstrates my ability to **design, implement, test, and debug UI Policies** in ServiceNow, applying ITIL-aligned Incident Management practices.  
The goal was to configure a UI Policy that triggers when an **Incident** is marked as *Resolved* or *Closed*, ensuring **data completeness, field control, and user guidance** before resolution.
- Enhance compliance with **ITIL-aligned Incident Management** processes.
---
##  Skills Demonstrated
- **ServiceNow ITSM Configuration** – UI Policy creation, field control, and user guidance.
- **ITIL Knowledge** – State-driven process compliance.
- **Testing & Debugging** – Ensuring functional accuracy.
- **User Experience Design** – Real-time form guidance to prevent errors.
- **Script Execution** (onCondition with g_form)
- Testing with Impersonation

---
## Objectives
- Automatically lock certain fields when resolution is selected.
- - Lock the **Urgency** and **Impact** fields.
- Require key fields to be completed before saving.
- - Make **Resolved By** mandatory.
- Provide **real-time information messages** for missing mandatory fields.
- - Display **Information Messages** when mandatory fields are missed.
- Improve resolution workflow to reduce errors and rework.

---

- Prevent post-resolution changes to priority-related fields.
- Ensure accountability by mandating the **Resolved By** field.
- Provide real-time, contextual guidance to users.
- Align ServiceNow workflows with ITIL best practices.

---

## Understanding the Concepts - ServiceNow & ITIL Concepts

### 1. **Incident Management**
Incident Management focuses on restoring normal service as quickly as possible while minimizing impact.  
At the **Resolved** or **Closed** state, correct classification and resolution details are critical for:
- SLA Compliance
- Reporting accuracy
- Root cause analysis
- Continuous improvement

---

### 2. **Change Management Relevance**
While this configuration is for **Incident Management**, the same UI Policy logic can be applied in **Change Management** to enforce state-based validation, ensuring approvals, testing evidence, and rollback plans are in place before closing a change.

---

### 3. **UI Policy in ServiceNow**
UI Policies dynamically change form behavior:
- Make fields **mandatory** or **read-only**.
- Show/hide fields.
- Display **information messages** to guide users.

Unlike Business Rules, UI Policies apply **before saving data**, enhancing user experience and reducing incorrect submissions.

---

## Implementation Details

### UI Policy Configuration

### Trigger Condition
- **Table:** `Incident [incident]`
- **Condition:** `State` is **Resolved** or **Closed**
- **Global:** Enabled  
- **Reverse if false:** Enabled (fields revert to editable when state changes away from Resolved/Closed)
- **On Load:** Enabled (triggers both on load and on change)

**Screenshot – UI Policy Condition Setup:**
![UI Policy Condition - Resolved or Closed](https://github.com/Gerald-Star/Incident-Resolved-Closed-UI-Policy-ServiceNow-Portfolio-Project/blob/main/1%20main%20State%20is%20Resoved%20or%20Closed.png?raw=true)

---

### UI Policy Actions
- **Urgency** → Read-only when Resolved/Closed  
- **Impact** → Read-only when Resolved/Closed  
- **Resolved by** → Mandatory  
- **Info Message:**  
  - Displays “Please complete all mandatory fields before saving this incident.”
  - Alerts the user if they attempt to save without completing required fields.

**Screenshot – UI Policy Action Setup:**
![UI Policy Actions - Urgency, Impact, Resolved by](3%20UI%20Policy%20Action%20showing%20Urgency%20etc.png)

---

### Alternative Setup for Extended Conditions
UI Policy can also include additional states (e.g., *Canceled*) if business rules require restrictions before full closure.

**Screenshot – Alternative Condition Setup:**
![Alternative Condition Setup](1%20state%20is%20resolved%20and%20closed.png)

---

In addition to controlling **Urgency**, the UI Policy also applies rules to the **Urgency**, **Resolved by** and **Impact** fields.

#### 1. Urgency - Read-only
- **Field:** `Urgency`
- **Read-only:** True
- **Purpose:** Locks urgency to preserve original priority level for SLA and reporting.

**Screenshot – Policy Action: Urgency**  
![UI Policy Action - Resolved by](resolved_by_screenshot.png)  

---

#### 2. Resolved by (Mandatory)
- **Field:** `Resolved by`
- **Mandatory:** True
- **Purpose:** Captures the resolver for accountability and historical tracking.

  
- **Behavior:** When the incident state is *Resolved* or *Closed*, the `Resolved by` field becomes **mandatory**.
- **Purpose:** This ensures that the system captures who resolved the incident for:
  - Accountability
  - Reporting
  - Knowledge base linkage
- **User Experience:** If a user tries to save the record without completing this field, an **information message** alerts them to the missing requirement.

**Screenshot – Policy Action: Resolved by**  
![UI Policy Action - Resolved by](resolved_by_screenshot.png)

---

#### 3. Impact (Read-only)
- **Behavior:** When the incident state is *Resolved* or *Closed*, the `Impact` field becomes **read-only**.
- **Purpose:** Prevents post-resolution modification of impact level, ensuring the historical accuracy of the incident's severity.
- **ITIL Relevance:** The impact level should reflect the state of the incident at the time it was addressed, which is crucial for trend analysis and SLA reporting.

**Screenshot – Policy Action: Impact**  
![UI Policy Action - Impact](impact_screenshot.png)

---

### Why These Actions Matter
Combining **Urgency (read-only)**, **Impact (read-only)**, and **Resolved by (mandatory)** enforces:
- **Data Integrity:** No critical classification fields can be altered after resolution.
- **Compliance:** Mandatory capture of resolver information.
- **Consistency:** Aligns with ITIL standards for Incident closure.


##  Testing & Debugging
1. Created incidents in various states.
2. Verified:
   - Fields lock when Resolved/Closed.
   - Mandatory “Resolved by” prompt before saving.
   - Info message appears on missing required fields.
3. Debugged using ServiceNow's UI Policy debugging tools to ensure no conflict with other scripts or policies.

---


## 💻 Scripting with onCondition()

To extend UI Policy functionality, a **Run Script** option was enabled:
**Purpose:**
When the condition is met, displays a reminder message instructing the user to complete resolution details before saving.

### onCondition() Script – True
```javascript
function onCondition() {
    var state = g_form.getValue('state');
    if (state == 'Resolved' || state == 'Closed') {
        g_form.addInfoMessage('Reminder: Populate the resolution information field before saving an incident in a resolved or closed state.');
    }
}

```


### onCondition() Script – False
**Purpose:**
When the state is changed away from Resolved or Closed, clears any previously displayed information messages.

```javascript
function onCondition() {
    g_form.clearMessages();
}
```

### Testing
**Steps:**

Impersonate "Abel Tutor" – Simulates a user with permission to set State to Resolved.

Open an Incident Record.

Change State to Resolved.

**Verify:**

Urgency and Impact become read-only.

Resolved By becomes mandatory.

Info message appears if mandatory fields are missing upon save.

Change State back to a non-resolved state.

**Confirm that:**

Fields are editable again.

Info messages are cleared.

---

##  Skills Demonstrated
- **ServiceNow ITSM Configuration** – UI Policy creation, field control, and user guidance.
- **ITIL Knowledge** – State-driven process compliance.
- **Testing & Debugging** – Ensuring functional accuracy.
- **User Experience Design** – Real-time form guidance to prevent errors.

---

##  Business Value
This configuration:
- Improves **data quality** for incident closure.
- Reduces post-resolution corrections.
- Aligns ITSM workflows with **ITIL best practices**.
- Enhances **audit readiness** and reporting accuracy.

---

##  Technologies & Tools
- **ServiceNow ITSM**
- **UI Policies**
- **Incident Management**
- **ITIL Framework**


