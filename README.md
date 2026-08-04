# SQL Injection & Web Vulnerabilities in Student Management System

## Overview

Multiple web vulnerabilities, including **SQL Injection (SQLi)**, were identified in the **Student Management System** within the `updatestudent.php` component.

The primary SQL Injection vulnerability arises from direct, unaligned concatenation of user-supplied input via the `GET` parameter `eno` into database queries without sanitization or parameterized statements.

This repository documents the root cause, technical proof of concept, impacted code segments, and recommended remediation guidance. The analysis was performed in an isolated laboratory environment for research and educational purposes.

---

## Vulnerability Information

| Field | Value |
| :--- | :--- |
| **Vulnerability Type** | SQL Injection (CWE-89)|
| **CWE** | CWE-89 |
| **Attack Vector** | Network / Remote |
| **Authentication Required** | Low / User (Requires minimum privileges to access `updatestudent.php`) |
| **Privileges Required** | Low (Basic student/user privileges) |
| **Vulnerable Component** | `updatestudent.php` |
| **Impact** | Unauthorized database access, session hijacking, data disclosure |
| **Severity** | **High** |
| **Submitter** | @hofuman |

---

## Vulnerable Endpoint & Parameters

* **Endpoint:** `/updatestudent.php`
* **HTTP Method:** `GET` / `POST`
* **Vulnerable Parameter:** `eno` (`?eno=1 OR 1=1`)

---

## Technical Analysis & Root Cause

### 1. SQL Injection (CWE-89)

An attacker with minimum user privileges accessing the `updatestudent.php` endpoint can exploit this vulnerability. The application assigns user input directly from `$_GET['eno']` to variable `$new3` without sanitization or parameterized queries:

```php
// Vulnerable Code Snippet
$new3 = $_GET['eno'];$sql = "select * from studenttable where Eno=$new3";
```

<img width="1161" height="573" alt="image" src="https://github.com/user-attachments/assets/99e4c253-228f-415b-b1d2-6a2c9e062f3e" />
