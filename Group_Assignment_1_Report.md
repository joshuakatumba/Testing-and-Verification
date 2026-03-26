# Group Assignment 1: Software Quality Attributes & System Testing

**Group Members:**
1. [Name 1] - [Student Number 1] - [Registration Number 1]
2. [Name 2] - [Student Number 2] - [Registration Number 2]
*(Add more rows as necessary)*

---

## Task 1: Software Quality Attributes (20 Points)

Based on the Software Requirements Specification (SRS) for the **"Ride With Me"** mobile application, the following five critical software quality attributes have been identified. These attributes are essential for ensuring that the system functions correctly, provides a good user experience, and meets the business objectives of a carpooling platform.

### 1. Performance
**Definition:** Performance defines how effectively the software responds to user actions within a specified timeframe and under certain workload conditions.
**Context in "Ride With Me":**
The SRS explicitly states that the system should respond to passenger queries and driver updates within 3 seconds, even when supporting up to 500 concurrent users. Fast response times are critical for user retention; when a passenger searches for an available ride, the filtering and retrieval of matching routes must happen almost instantaneously. Similarly, drivers need immediate confirmation when they post a travel route or receive a booking request. Poor performance can lead to missed rides or double bookings, directly impacting the core business flow.

### 2. Security
**Definition:** Security measures the system's ability to protect sensitive data and prevent unauthorized access or malicious actions.
**Context in "Ride With Me":**
Security is a paramount concern for a carpooling application. The SRS mandates driver verification by system administrators to build trust. Furthermore, the application handles sensitive personal data, including user profiles (names, contacts), journey histories, and potentially payment details. This data must be encrypted during transmission and storage. Additionally, authentication and authorization mechanisms are required to ensure that only logged-in passengers can book seats and only authorized drivers can manage their posted routes.

### 3. Availability
**Definition:** Availability represents the proportion of time the system is functional and accessible to users when they need it.
**Context in "Ride With Me":**
The application requires a 99.9% system uptime as per the SRS. Since journeys can be booked or searched for at any time of the day or night, the system must remain highly available. If the application experiences frequent downtime, passengers cannot find rides when they need them urgently, and drivers cannot advertise sudden trips. High availability is achieved through robust server infrastructure, fault tolerance, and effective database management to ensure travel data is always accessible.

### 4. Usability
**Definition:** Usability measures how easy it is for users to navigate the system, understand its features, and achieve their goals without confusion.
**Context in "Ride With Me":**
The primary actors (passengers and drivers) are likely everyday people, not necessarily technical experts. Therefore, the user interface must be intuitive. Passengers must be able to easily search for pick-up/drop-off locations, view available seats, and book a ride. Drivers need a straightforward way to add route details (time, capacity). A complex or cluttered interface would lead to abandoned bookings or improperly configured routes. Features like an intuitive rating system further enhance usability by keeping the feedback mechanism simple.

### 5. Portability (Cross-Platform Compatibility)
**Definition:** Portability is the degree to which a software system can be executed on different operating environments without major modifications.
**Context in "Ride With Me":**
The SRS specifies that the mobile application must be supported on both Android and iOS platforms. Given that the target market consists of smartphone users, restricting the app to a single platform would severely limit its user base and the pool of available drivers/passengers. Ensuring high portability—perhaps by using cross-platform development frameworks—allows the app to seamlessly serve a diverse audience regardless of the mobile device they own.

---

## Task 2: System Test Design for `tail` Command (15 Points)

This task focuses on designing system test cases for the `tail` command-line utility based on its documentation snippet. The functionality aims to output the last part of files to standard output.

### Test Data Preparation:
Assume we have the following text file named `example_log.txt`:
```
Line 1: System initiated...
Line 2: Loading configuration...
Line 3: Database connection established.
Line 4: User authentication successful for UID: 101.
Line 5: Fetching user profile data.
Line 6: Image resources failed to load. (ERROR: 404)
Line 7: Retrying image fetch...
Line 8: Image resources loaded successfully.
Line 9: User initiated log out.
Line 10: System shutting down safely.
```

### System Test Cases

| TC ID | Test Scenario / Objective | Pre-conditions | Test Steps (Command) | Expected Output / Behavior | Status (Pass/Fail) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-TAIL-01** | **Default Execution (No parameters)**<br>Verify that running `tail` without any parameters prints the last 10 lines of the specified file. | `example_log.txt` exists and has at least 10 lines. | `tail example_log.txt` | The system outputs all 10 lines of `example_log.txt` to the standard output. | |
| **TC-TAIL-02** | **Line Count Execution (`-n`, `--lines=K`)**<br>Verify that the tool correctly outputs a specific number of lines (`K`). | `example_log.txt` exists. | `tail --lines=4 example_log.txt` | Output consists of only Lines 7 to 10:<br>`Line 7: Retrying...`<br>`Line 8: Image resources...`<br>`Line 9: User initiated...`<br>`Line 10: System shutting...` | |
| **TC-TAIL-03** | **Line Output from Specific Line (`-n +K`)**<br>Verify that using the `+K` modifier starts output from the Kth line. | `example_log.txt` exists. | `tail -n +8 example_log.txt` | Output starts precisely at Line 8 and continues to Line 10. | |
| **TC-TAIL-04** | **Byte Count Output (`-c`, `--bytes=K`)**<br>Verify that the tool outputs exactly `K` bytes starting from the end of the file. | `example_log.txt` exists. | `tail --bytes=20 example_log.txt` | Output shows exactly the last 20 characters/bytes of the file (e.g., `"uting down safely.\n"`). | |
| **TC-TAIL-05** | **Continuous Follow (`-f`, `--follow[=name]`)**<br>Verify that the command does not terminate but continuously waits and prints appended data as the file grows. | `example_log.txt` exists in a separate terminal process appending data. | `tail --follow example_log.txt` | Output displays the last 10 lines initially. When another process appends a new line (e.g., `Line 11: Restarting...` to the file, `tail` immediately outputs `Line 11` without the command exiting. | |
| **TC-TAIL-06** | **Process ID Termination (`--pid=PID`)**<br>Verify that `tail` gracefully exits when used in conjunction with `-f` if the specified process ID dies. | `example_log.txt` exists. Another process (PID `1234`) is running. | `tail -f --pid=1234 example_log.txt` | `tail` initially follows the file. When process `1234` is explicitly killed/terminated, the `tail` command also terminates and returns to the shell prompt. | |
| **TC-TAIL-07** | **Retry Mechanism (`--retry`)**<br>Verify that `tail` keeps trying to open a file when used with follow (`-f`) if it is currently inaccessible or deleted and recreated. | `dynamic_log.txt` does not initially exist or permission is temporarily denied. | `tail -f --retry dynamic_log.txt` | `tail` prints an initial warning/error but remains active. When `dynamic_log.txt` is created and data is echoed into it, `tail` subsequently outputs the appended data. | |
| **TC-TAIL-08** | **Negative Test (Invalid Line Count)**<br>Verify that the tool handles invalid string input for the line count parameter. | `example_log.txt` exists. | `tail --lines="abc" example_log.txt` | Command fails gracefully, returns an error message like `invalid number of lines: 'abc'`, and terminates without printing file contents. | |

---

## Task 3: System Test Design for Dummy REST API (15 Points)

This task focuses on designing 5 test cases for the Dummy REST API (`http://dummy.restapiexample.com/`) to ensure the basic CRUD operations function correctly according to standard REST principles.

### API Test Cases

| TC ID | API Endpoint | HTTP Method | Test Scenario / Objective | Request Body / Payload | Expected Response (Code & Body snippet) | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-API-01** | `/api/v1/employees` | `GET` | **Retrieve All Employees**<br>Verify that the endpoint successfully returns a list of all employee records. | *None* | **Status:** `200 OK`<br>**Body:** `{"status": "success", "data": [{...}, {...}], "message": "Successfully! All records has been fetched."}` | |
| **TC-API-02** | `/api/v1/employee/1` | `GET` | **Retrieve Single Employee**<br>Verify that the endpoint successfully returns data for a specific valid employee ID. | *None* | **Status:** `200 OK`<br>**Body:** `{"status": "success", "data": {"id": 1, "employee_name": "Tiger Nixon", ...}, "message": "Successfully! Record has been fetched."}` | |
| **TC-API-03** | `/api/v1/create` | `POST` | **Create New Employee**<br>Verify that a new employee record can be created with valid JSON payload. | `{"name":"John Doe","salary":"50000","age":"30"}` | **Status:** `200 OK` (or `201 Created`)<br>**Body:** `{"status": "success", "data": {"name": "John Doe", "salary": "50000", "age": "30", "id": 5462}, "message": "Successfully! Record has been added."}` | |
| **TC-API-04** | `/api/v1/update/21` | `PUT` | **Update Existing Employee**<br>Verify that an existing employee record can be updated securely with a well-formed JSON payload. | `{"name":"Tiger Nixon","salary":"350000","age":"62"}` | **Status:** `200 OK`<br>**Body:** `{"status": "success", "data": {"name": "Tiger Nixon", "salary": "350000", "age": "62"}, "message": "Successfully! Record has been updated."}` | |
| **TC-API-05** | `/api/v1/delete/2` | `DELETE` | **Delete Existing Employee**<br>Verify that an existing employee record can be successfully deleted using its ID. | *None* | **Status:** `200 OK`<br>**Body:** `{"status": "success", "data": "2", "message": "Successfully! Record has been deleted"}` | |

---
*End of Report*
