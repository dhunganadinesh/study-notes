# 🛡️ Security Governance Notes
## 🔺 CIA Triad

#### 🕵️‍♂️ Confidentiality
Measure to ensure the protection of secrecy of data, objects, or resources.
- Minimize unauthorized access to data.

Countermeasure
1. Encryption
Data at Rest (symmetric encryption).
Data in 
2. Network traffice padding
3. Strice access control
4. Rigorous authentication
5. Data classification
6. Extensive perosnnel training

### ✅ Integrity 
Concept of protecting the reliability and correctness of data.

- Prevents unauthorized subjets from making modification.
- Preventing authorized subjects from making unauthorized modification (such as mistakes.)
*Accuracy, Truthfulness, Validity, Accountbilit, Responsibility, Completeness, Comprehensiveness*

### 📶 Availability 
Authorized subjects are granted timely and uninterrupted access to objects.
Uninterruptes Access.

Countermeasures
1. Ensure authorized access and acceptable level of performance
2. Handel interruptions.
3. Provide redundancy.
4. Maintain reliable backups.
5. Prevent data loss.

### Non-repudation: Ensure subject of an activity or who caused an event cannot  deny that the event occured.
*Tools: Digital certificate, Session identifier, transaction logs*

### Authenticity: Data is authentic or genuine and originates from its alleged source.

#### Five elements of AAA
1. Identification: Claiming to be an identity when attempting to access a secured area or system.
2. Authentication: Proving that you are that claimed identity.
3. Autorization: Defining permissino or a resourse and object access for specific identity or subject.
4. Auditing: Recording of events.
5. Accounting: Reviewing log files to check for compliance.

# 🛡️ Protection Mechanisms

- 🔒 Encryption

- 🔐 Defense in depth: Multiple controls in a series.
**Security solution**
☑️ Using layer in series (strong)
✖️ Using layer in parallel (shallow)

- 📦Abstraction: Efficient. Similar elements are put together. *HR, Payroll, IT*
- 🧥 Data hiding: Security through obscurity does not provide protection. Hoping no one discovers.

# 🚧 Security Boundaries

Security boundaries define the points in a system where trust levels change, separating different zones to protect assets and control access.

---

## 🔑 Purpose of Security Boundaries

- Limit the scope of trust between system components
- Prevent unauthorized access or data leakage across zones
- Enforce security policies appropriate to each zone
- Contain potential security breaches within a boundary

---

## 🏗️ Types of Security Boundaries

- **Physical Boundary**  
  Separates physical environments, e.g., a locked server room.

- **Network Boundary**  
  Divides networks using firewalls, VLANs, or DMZs.

- **Logical Boundary**  
  Segregates data or processes within a system, e.g., user permissions, sandboxing.

- **Application Boundary**  
  Interfaces where applications communicate, enforcing validation and authentication.

---

## 🔄 Examples

| Boundary Type     | Example                        |
|-------------------|--------------------------------|
| Physical          | Data center door access control|
| Network           | Firewall between internal and external networks |
| Logical           | User role separation within a database |
| Application       | API authentication tokens      |

---

## 🛡️ Importance

- Prevents attackers from moving laterally within a system
- Ensures sensitive data is protected according to its classification
- Supports compliance with regulatory requirements

---

## 📈 Best Practices

- Clearly define and document all security boundaries
- Use multiple layers of controls at boundaries (defense in depth)
- Regularly audit boundary controls and configurations
- Implement strict monitoring and logging at boundaries

---

*End of notes on Security Boundaries.*

