# OFFY Setup & Git Configuration Guide

A simple, step-by-step guide to installing the **OFFY Connector** in **Odoo 19 Enterprise**, pushing code updates to Git, understanding the folder layout, and configuring your Odoo server.

---

## 1. Clone the Repository

Open your terminal, go to your Odoo installation directory, and clone the **`V19.0-E`** branch:

```bash
cd ~/odoo19.0/odoo19
git clone -b V19.0-E https://github.com/odoxsofthub/offy.git
```

To verify you are on the right branch:
```bash
cd offy
git status
```
*Output: `On branch V19.0-E`*

---

## 2. Adding the Module to Git & Pushing to Server

To add the module to Git under your own custom branch and push it to the server, follow these steps:

### 1. Check changed files
```bash
cd ~/odoo19/offy
git status
```

### 2. Stage your changes
```bash
git add offy_mobile_connector_odoo/
```

### 3. Commit your changes
```bash
git commit -m "[UPDT] offy module update"
```

### 4. Push to remote
```bash
git push origin custom_branch
```
*(Replace `custom_branch` with your active branch name)*

When prompted in the terminal:

**1. Enter your GitHub username:**
```text
Username for 'https://github.com': your_username
```

**2. Enter your GitHub password:**
```text
Password for 'https://your_username@github.com': your_password
```

Your updates are now pushed to GitHub!

---

## 3. Directory Structure

Inside the cloned `offy` folder, you will see:

```text
offy/
├── docs/                             # Documentation and images
├── offy_mobile_app/                  # Flutter mobile app
└── offy_mobile_connector_odoo/       # Odoo modules
    ├── odx_mobile_api/               # REST API module
    └── odx_mobile_connector/         # OFFY Connector module
```

> [!NOTE]
> The folder you need to add to Odoo is **`offy_mobile_connector_odoo`** (because it directly contains the Odoo modules).

---

## 4. Update `odoo.conf`

Open your `odoo.conf` file in your editor (such as PyCharm or VS Code).

### A. Configure `addons_path`

Add the path of `offy_mobile_connector_odoo` to your existing `addons_path` separated by a comma:

```ini
addons_path = /home/odx-11/odoo19.0/odoo19/addons,/home/odx-11/odoo19.0/odoo19/enterprise-19.0,/home/odx-11/odoo19.0/odoo19/offy/offy_mobile_connector_odoo
```

#### If you have a project addons folder:
Simply add the path to the end of your list:
```ini
addons_path = /path/to/addons,/path/to/enterprise-19.0,/path/to/your_project_addons,/path/to/offy/offy_mobile_connector_odoo
```

*(Alternatively, you can just copy the `odx_mobile_connector` and `odx_mobile_api` folders directly into your existing project addons folder).*

---

### B. Configure `dbfilter`

Add or update the `dbfilter` line with your database name:

```ini
dbfilter = ^offy_enterprise$
```

- **Why is this needed?** It tells Odoo to automatically open your specific database so the mobile app connects smoothly without asking you to choose a database.
- **Your database name**: Replace `offy_enterprise` with your actual database name (for example, `^my_company_db$`).

---

### C. Example `odoo.conf`

```ini
[options]
admin_passwd = 123
db_host = localhost
db_port = 5432
db_user = odoo19
db_password = 123
http_port = 8019

dbfilter = ^offy_enterprise$

addons_path = /home/odx-11/odoo19.0/odoo19/addons,/home/odx-11/odoo19.0/odoo19/enterprise-19.0,/home/odx-11/odoo19.0/odoo19/offy/offy_mobile_connector_odoo
```

> [!IMPORTANT]
> **Restart your Odoo server** after saving `odoo.conf` so the changes take effect.
