# Basic Linux Permissions: `chmod 775`
The command `chmod 775` is used to change the permissions of a file or directory in Linux or Unix-based systems. It sets the **read**, **write**, and **execute** permissions for the **owner** and the **group**, while granting **read** and **execute** permissions to **others**. 

### **Detailed Breakdown**
- `7` (Owner): Read (4) + Write (2) + Execute (1) = **7** (full access)
- `7` (Group): Read (4) + Write (2) + Execute (1) = **7** (full access)
- `5` (Others): Read (4) + Execute (1) = **5** (limited access)

---

### **Usage**
1. **Command Syntax**:
   ```bash
   chmod 775 <filename or directory>
   ```

2. **Example**:
   To apply `775` permissions to a file named `example.sh`:
   ```bash
   chmod 775 example.sh
   ```

3. **Effect**:
   - **Owner** (User who owns the file): Can read, write, and execute.
   - **Group** (Users in the same group as the owner): Can read, write, and execute.
   - **Others** (Everyone else): Can read and execute, but **cannot write**.

---

### **Verify Permissions**
You can check the updated permissions using the `ls -l` command:
```bash
ls -l example.sh
```
Output:
```
-rwxrwxr-x 1 user group 1234 Jan 24 10:00 example.sh
```
- **`rwx` (Owner)**: Read, write, execute.
- **`rwx` (Group)**: Read, write, execute.
- **`r-x` (Others)**: Read, execute.

---

### **When to Use `chmod 775`**
- When you want to grant full permissions to the file's owner and group, while restricting write access for others.
- Commonly used for directories where multiple users in the same group need access to modify files but want to prevent unauthorized modifications by others. 
