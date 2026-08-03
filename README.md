# CVE-2026-XXXX – SQL Injection & Web Vulnerabilities in Student Management System

## Overview

Multiple web vulnerabilities, including **SQL Injection (SQLi)**, **Reflected/Stored Cross-Site Scripting (XSS)**, and **Insecure Credential Storage**, were identified in the **Student Management System** within the `updatestudent.php` component.

The primary SQL Injection vulnerability arises from direct, unaligned concatenation of user-supplied input via the `GET` parameter `eno` into database queries without sanitization or parameterized statements.

This repository documents the root cause, technical proof of concept, impacted code segments, and recommended remediation guidance. The analysis was performed in an isolated laboratory environment for research and educational purposes.

---

## Vulnerability Information

| Field | Value |
| :--- | :--- |
| **CVE** | CVE-2026-XXXX |
| **Vulnerability Type** | SQL Injection (CWE-89), Cross-Site Scripting (CWE-79), Cleartext Storage of Sensitive Information (CWE-312) |
| **CWE** | CWE-89, CWE-79, CWE-312 |
| **Attack Vector** | Network / Remote |
| **Authentication Required** | Depends on deployment |
| **Vulnerable Component** | `updatestudent.php` |
| **Impact** | Unauthorized database access, session hijacking, data disclosure |
| **Severity** | **High** |

---

## Vulnerable Endpoint & Parameters

* **Endpoint:** `/updatestudent.php`
* **HTTP Method:** `GET` / `POST`
* **Vulnerable Parameter:** `eno` (`?eno=1 OR 1=1`)

---

## Technical Analysis & Root Cause

### 1. SQL Injection (CWE-89)

The application assigns user input directly from `$_GET['eno']` to variable `$new3` without sanitization or parameterized queries:

```php
// Vulnerable Code Snippet
$new3 = $_GET['eno'];$sql = "select * from studenttable where Eno=$new3";
