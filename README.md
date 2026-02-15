# oracle_pdb_ass_II_28763_eddy
Oracle PDB Management Assignment - Database Development with PL/SQL
# Oracle Pluggable Database (PDB) Management Assignment

**Course:** Database Development with PL/SQL (INSY 8311)  
**Student Name:** Eddy  
**Student ID:** 28763  
**Assignment:** Individual Assignment II  
**Submission Date:** February 16, 2026

---

## Assignment Overview

This assignment demonstrates practical skills in Oracle Multitenant Architecture, focusing on:
- Creating and managing Pluggable Databases (PDBs)
- User creation and management within PDBs
- PDB lifecycle operations (creation and deletion)
- Oracle Enterprise Manager (OEM) configuration and usage
- Professional technical documentation

---

## Oracle Environment

**Database Version:** Oracle 21c Express Edition (21.3.0.0.0)  
**Platform:** Microsoft Windows x86 64-bit  
**Container Database (CDB):** XE  
**Oracle Home:** C:\APP\CHRIS\PRODUCT\21C\  
**Enterprise Manager Port:** 5501 (HTTPS)

---

## Task 1: Create a New Pluggable Database

### Objective
Create a permanent PDB with proper naming conventions for future class work.

### Implementation

**PDB Name:** `Ed_pdb_28763`  
**Admin Username:** `Eddy_plsqlauca_28763`  
**Password:** [Configured securely]

### Commands Executed

```sql
-- Connect as SYSDBA
CONN / AS SYSDBA

-- Create the Pluggable Database
CREATE PLUGGABLE DATABASE Ed_pdb_28763
    ADMIN USER Eddy_plsqlauca_28763 IDENTIFIED BY 12345
    FILE_NAME_CONVERT = ('C:\APP\CHRIS\PRODUCT\21C\ORADATA\XE\PDBSEED\',
                         'C:\APP\CHRIS\PRODUCT\21C\ORADATA\XE\Ed_pdb_28763\');

-- Open the PDB
ALTER PLUGGABLE DATABASE Ed_pdb_28763 OPEN;

-- Verify PDB creation and status
SELECT name, open_mode FROM v$pdbs;

-- Switch to the PDB context
ALTER SESSION SET CONTAINER = Ed_pdb_28763;

-- Verify the admin user exists
SELECT username, account_status FROM dba_users WHERE username = 'EDDY_PLSQLAUCA_28763';
```

### Results
- ✅ PDB `Ed_pdb_28763` created successfully
- ✅ PDB opened in READ WRITE mode
- ✅ Admin user `Eddy_plsqlauca_28763` created and active
- ✅ User account ready for future class work

### Evidence
See screenshots folder: 

---

## Task 2: Create and Delete a Temporary PDB

### Objective
Demonstrate PDB lifecycle management by creating and then completely removing a temporary PDB.

### Implementation

**Temporary PDB Name:** `Ed_to_delete_pdb_28763`

### Commands Executed

```sql
-- Create temporary PDB
CREATE PLUGGABLE DATABASE Ed_to_delete_pdb_28763
    ADMIN USER temp_admin IDENTIFIED BY temp123
    FILE_NAME_CONVERT = ('C:\APP\CHRIS\PRODUCT\21C\ORADATA\XE\PDBSEED\',
                         'C:\APP\CHRIS\PRODUCT\21C\ORADATA\XE\Ed_to_delete_pdb_28763\');

-- Open the temporary PDB
ALTER PLUGGABLE DATABASE Ed_to_delete_pdb_28763 OPEN;

-- Verify it exists
SELECT name, open_mode FROM v$pdbs WHERE name = 'ED_TO_DELETE_PDB_28763';

-- Close the PDB before deletion
ALTER PLUGGABLE DATABASE Ed_to_delete_pdb_28763 CLOSE IMMEDIATE;

-- Drop the PDB including datafiles
DROP PLUGGABLE DATABASE Ed_to_delete_pdb_28763 INCLUDING DATAFILES;

-- Verify deletion (should return no rows)
SELECT name FROM v$pdbs WHERE name = 'ED_TO_DELETE_PDB_28763';
```

### Results
- ✅ Temporary PDB created successfully
- ✅ PDB verified to exist in container database
- ✅ PDB closed properly before deletion
- ✅ PDB and all associated datafiles deleted completely
- ✅ Confirmed PDB no longer exists in v$pdbs

### Evidence
See screenshots folder: `task2_temp_pdb_creation.png`, `task2_pdb_exists.png`, `task2_pdb_deletion.png`, `task2_pdb_deleted_confirmation.png`

---

## Task 3: Oracle Enterprise Manager (OEM)

### Objective
Configure and access Oracle Enterprise Manager Database Express to monitor and manage the Oracle environment.

### Configuration

**Access URL:** `https://localhost:5501/em`  
**Login Credentials:** SYS user with SYSDBA privileges  
**Container Context:** Ed_pdb_28763

### Setup Steps

```sql
-- Configure HTTPS port for OEM
EXEC DBMS_XDB_CONFIG.SETHTTPSPORT(5501);

-- Verify port configuration
SELECT DBMS_XDB_CONFIG.GETHTTPSPORT() FROM DUAL;

-- Ensure PDB is open
ALTER PLUGGABLE DATABASE Ed_pdb_28763 OPEN;
```

### Dashboard Features Verified
- Database instance status and uptime
- PDB container information (Ed_pdb_28763)
- Performance metrics and activity monitoring
- Resource utilization (CPU, memory, storage)
- Database version and platform details

### Results
- ✅ OEM accessible at configured port
- ✅ Dashboard displays correct PDB context
- ✅ All completed tasks reflected in the environment
- ✅ Username and database information visible

### Evidence
See screenshots folder: `task3_oem_dashboard.png`

---

## Challenges Encountered and Solutions

### Challenge 1: Port Conflicts
**Issue:** Initial attempts to configure OEM on ports 5500 and 8080 resulted in port conflict errors (ORA-44718, ORA-12542).

**Solution:** 
- Identified that port 5500 was occupied by existing Enterprise Manager instance
- Port 8080 was in use by another service
- Successfully configured OEM on alternative port 5501
- Verified port availability before configuration

### Challenge 2: TNS Connection Issues
**Issue:** Encountered ORA-12154 error when attempting to connect directly to PDB using TNS alias.

**Solution:**
- Used `ALTER SESSION SET CONTAINER` method instead
- Connected as SYSDBA first, then switched container context
- This approach proved more reliable for PDB management tasks

### Challenge 3: File Path Conflicts
**Issue:** Initial PDB creation attempt failed with ORA-01537 error due to file path conflicts.

**Solution:**
- Ensured each PDB has its own unique directory
- Used PDB-specific naming in FILE_NAME_CONVERT clause
- Verified no existing files before creation

---

## Key Learnings

1. **Oracle Multitenant Architecture:** Understanding the relationship between CDB and PDBs is crucial for effective database management.

2. **PDB Lifecycle Management:** Proper sequence matters - PDBs must be closed before deletion, and datafiles should be explicitly removed.

3. **Naming Conventions:** Strict adherence to naming standards ensures consistency and prevents conflicts in multi-PDB environments.

4. **Enterprise Manager:** OEM provides comprehensive monitoring and management capabilities, essential for database administration.

5. **Troubleshooting Skills:** Port conflicts and connection issues require systematic diagnosis and alternative approaches.

---

## Academic Integrity Statement

I, **Eddy (Student ID: 28763)**, hereby declare that:

- This assignment is entirely my own work
- All commands were executed personally on my Oracle environment
- All screenshots are genuine captures from my system
- No content was copied from classmates or external sources
- No AI tools were used to generate commands or solutions
- I have followed all assignment instructions and naming conventions
- I understand that any violation of academic integrity will result in zero marks

**Date:** February 13, 2026  
**Signature:** Eddy

---

## Repository Structure

```
oracle_pdb_ass_II_28763_eddy/
│
├── README.md                              # This file
│
└── screenshots/
    ├── task1_pdb_creation.png            # PDB creation command
    ├── task1_pdb_open.png                # PDB open status
    ├── task1_user_created.png            # User verification
    ├── task2_temp_pdb_creation.png       # Temporary PDB created
    ├── task2_pdb_exists.png              # Verification of existence
    ├── task2_pdb_deletion.png            # Deletion command
    ├── task2_pdb_deleted_confirmation.png # Deletion confirmed
    └── task3_oem_dashboard.png           # OEM dashboard view
```

---

## Submission Information

**Repository Link:** https://github.com/gatesieddyguerra-spec/oracle_pdb_ass_II_28763_eddy  
**PDB Name Created:** Ed_pdb_28763  
**Issues Encountered:** Yes (Port conflicts and TNS connection issues - resolved)  
**Submission Date:** February 13, 2026  
**Status:** Completed before deadline ✅

---

## References

- Oracle Database 21c Documentation
- Oracle Multitenant Administrator's Guide
- Oracle Enterprise Manager Database Express Guide
- Course lecture notes and laboratory materials

---

**End of Documentation**
