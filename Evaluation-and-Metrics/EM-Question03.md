# EM - Question 03 - Name three key quality attributes often evaluated in software architectures, and provide a brief example for each.

Three key quality attributes often evaluated in software architectures are **performance**, **security**, and **usability**. These attributes, sometimes called "ilities" in the industry, are non-functional requirements that are critical to a system's success.

---

### 1. Performance
**Performance** is the system's ability to execute tasks and respond to user requests efficiently and within a specific time frame. It's often measured in terms of throughput, latency, and resource utilization. A well-performing architecture ensures that the system can handle its workload without unacceptable delays or consuming excessive resources.

* **Example:** For a financial trading platform, a critical performance requirement is a **latency** of less than 10 milliseconds for a trade to be executed. The architecture would need to be designed with low-latency messaging queues, optimized data access patterns, and geographically distributed servers to ensure trades are processed as quickly as possible, even during peak trading hours. 

---

### 2. Security
**Security** is the architecture's ability to protect the system and its data from unauthorized access, use, disclosure, disruption, modification, or destruction. It involves designing a system with robust authentication, authorization, data encryption, and logging mechanisms to prevent threats and ensure data integrity and confidentiality.

* **Example:** An e-commerce platform needs to protect customer data, including personal information and payment details. The architecture would require **end-to-end data encryption** from the user's browser to the database. It would also implement a robust **Identity and Access Management (IAM)** system to ensure only authorized personnel can access sensitive data and that customers can't access each other's accounts.

---

### 3. Usability
**Usability** is the ease with which users can learn, operate, and be effective with a system. It's often influenced by the architecture's ability to support a responsive and intuitive user interface. While often seen as a user interface (UI) concern, architectural decisions significantly impact a system's usability.

* **Example:** Consider a word processing application with a robust **"undo" feature**. From an architectural perspective, this isn't just a simple UI button. It requires a Command pattern to be implemented, where each user action is recorded as a command object that can be stored in a history stack. The architecture must support this history, allowing the user to easily revert previous actions, which directly improves the user's experience and error recovery.