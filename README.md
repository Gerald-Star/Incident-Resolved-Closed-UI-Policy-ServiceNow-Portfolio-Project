
# ServiceNow Incident Management: UI Policy for Resolving and Closing Incidents via Client-Side API


## Project Overview
This project demonstrates my ability to **design, implement, test, and debug UI Policies** in ServiceNow, applying ITIL-aligned Incident Management practices.  
The goal is to demonstrate and configure a UI Policy that triggers when an **Incident** is marked as *Resolved* or *Closed*, ensuring **data completeness, field control, and user guidance** before resolution.
- Demonstrates UI Policy Actions triggered when an incident’s State is set to Resolved or Closed.
The UI policy ensures that specific fields are either locked (read-only) or made mandatory to maintain data integrity before resolution.
- Enhance compliance with **ITIL-aligned Incident Management** processes.
- Displays an Information Message prompting the user if they miss a mandatory field, only after they attempt to save.
---
![Incident /Resolved Closed UI Policy](https://github.com/Gerald-Star/Incident-Resolved-Closed-UI-Policy-ServiceNow-Portfolio-Project/blob/main/Banner%20.png?raw=true)
---
##  Skills Demonstrated
- **ServiceNow ITSM Configuration** – UI Policy creation, field control, and user guidance.
- **ITIL Knowledge** – State-driven process compliance.
- **Testing & Debugging** – Ensuring functional accuracy.
- **User Experience Design** – Real-time form guidance to prevent errors.
- **Script Execution** (onCondition with g_form)
- Testing with Impersonation
---

In the world of IT service management, efficiently handling incidents is crucial for maintaining smooth operations and ensuring customer satisfaction. ServiceNow provides a robust platform for managing incidents, and one of the key features is its ability to streamline processes through User Interface (UI) policies. Implementing the ServiceNow UI policies in the context of ServiceNow Incident Management is a powerful tool that allows you to enforce specific behaviours on forms and lists without needing complex scripting.

## Objectives
- Automatically lock certain fields when resolution is selected.
- - Lock the **Urgency** and **Impact** fields.
- Require key fields to be completed before saving.
- - Make **Resolved By** mandatory.
- Provide **real-time information messages** for missing mandatory fields.
- - Display **Information Messages** when mandatory fields are missed.
- Improve resolution workflow to reduce errors and rework.

---
By leveraging the Client-Side API, you can enhance user experience and automate various tasks related to resolving and closing incidents. Imagine a scenario where your support team faces a barrage of incidents daily. With well-crafted UI policies, you can automatically hide, show, or make fields mandatory based on the incident's status. For instance, once an incident is marked as "Resolved," you could auto-hide fields irrelevant to that status or prompt users for additional resolution details, ensuring that all necessary information is captured before the incident is closed. 

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
By effectively implementing these UI policies, you not only enhance your incident management process's efficiency but also provide your team with a structured approach to managing their workload. Additionally, this results in quicker resolution times, better documentation, and higher overall user satisfaction. In summary, understanding how to create and implement UI policies while leveraging the capabilities of the Client-Side API can greatly transform your incident management approach, making it more effective and user-friendly.

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

**Screenshot – UI Policy Condition Setup: Resolved or Closed**
![UI Policy Condition - Resolved or Closed](https://github.com/Gerald-Star/Incident-Resolved-Closed-UI-Policy-ServiceNow-Portfolio-Project/blob/main/1%20main%20State%20is%20Resoved%20or%20Closed.png?raw=true)

---

### UI Policy Actions
- **Urgency** → Read-only when Resolved/Closed  
- **Impact** → Read-only when Resolved/Closed  
- **Resolved by** → Mandatory  
- **Info Message:**  
  - Displays “Please complete all mandatory fields before saving this incident.”
  - Alerts the user if they attempt to save without completing required fields.

**Screenshot – UI Policy Action Setup: Urgency, Impact, Resolved by**
![UI Policy Actions - Urgency, Impact, Resolved by](https://github.com/Gerald-Star/Incident-Resolved-Closed-UI-Policy-ServiceNow-Portfolio-Project/blob/main/7%20Two%20more%20Policy%20Action-Resolved%20by%20and%20impact.png?raw=true)

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
![UI Policy Action - Urgency](https://github.com/Gerald-Star/Incident-Resolved-Closed-UI-Policy-ServiceNow-Portfolio-Project/blob/main/2%20Add%20UI%20Policy%20Action.png?raw=true)  

---
**Screenshot - UI Policy Action set to Read only**
![UI Policy Action - set to Read only](https://github.com/Gerald-Star/Incident-Resolved-Closed-UI-Policy-ServiceNow-Portfolio-Project/blob/main/3%20UI%20Policy%20Action%20showing%20Urgency%20etc.png?raw=true)

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

**Screenshot – Policy Action: Resolved by setting Mandatory to True**  
![UI Policy Action - Resolved by](https://github.com/Gerald-Star/Incident-Resolved-Closed-UI-Policy-ServiceNow-Portfolio-Project/blob/main/6%20Adding%20Policy%20Action%20Resolved%20By.png?raw=true)

---

#### 3. Impact (Read-only)
- **Behavior:** When the incident state is *Resolved* or *Closed*, the `Impact` field becomes **read-only**.
- **Purpose:** Prevents post-resolution modification of impact level, ensuring the historical accuracy of the incident's severity.
- **ITIL Relevance:** The impact level should reflect the state of the incident at the time it was addressed, which is crucial for trend analysis and SLA reporting.

**Screenshot – Policy Action: Impact**  
![UI Policy Actions - Impact](https://github.com/Gerald-Star/Incident-Resolved-Closed-UI-Policy-ServiceNow-Portfolio-Project/blob/main/7%20Two%20more%20Policy%20Action-Resolved%20by%20and%20impact.png?raw=true)

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

![First Scritpting with no Execute if false](https://github.com/Gerald-Star/Incident-Resolved-Closed-UI-Policy-ServiceNow-Portfolio-Project/blob/main/8%20Run%20Scripto%20to%20execute%20when%20condition%20is%20true.png?raw=true)

### onCondition() Script – False
**Purpose:**
When the state is changed away from Resolved or Closed, it clears any previously displayed information messages.

```javascript
function onCondition() {
    g_form.clearMessages();
}
```

![Second Scripting-Execute if false](https://github.com/Gerald-Star/Incident-Resolved-Closed-UI-Policy-ServiceNow-Portfolio-Project/blob/main/10%20run%20script%20false.png?raw=true)
### Testing Incident Use Case
**Steps:**

**Impersonate "Abel Tutor" – Simulates a user with permission to set State to Resolved.**

Open an Incident Record.

Change State to Resolved.

**Verify Effect:**

![State:Resolved/Closed](https://github.com/Gerald-Star/Incident-Resolved-Closed-UI-Policy-ServiceNow-Portfolio-Project/blob/main/State%20Resolved%20Closed.png?raw=true)

Urgency and Impact become read-only.

Resolved By becomes mandatory.

An info message appears if mandatory fields are missing upon save.

If the state changes back (eg, In Progress), messages are cleared. 

Change the State back to a non-resolved state.

![Incident Resolved - In Progress](https://github.com/Gerald-Star/Incident-Resolved-Closed-UI-Policy-ServiceNow-Portfolio-Project/blob/main/11%20After%20setting%20script%20false%20you%20can%20select%20state%20In%20progress%20with%20no%20infoMessage%20appearing.png?raw=true)

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


