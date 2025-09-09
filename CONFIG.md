# nexFluxo Configuration Guide

This document explains how to configure nexFluxo using the **AppConfig.xml** file and the optional **external configuration files**.  
The main configuration is stored in `AppConfig.xml`, while extensions can be placed in separate files under `config_extended/`.

---

## 1) General Structure

The master file is `AppConfig.xml`. It contains:

- **runtime**: mode, paths, log level, UI options, jobs  
- **database**: provider and connection string  
- **authorization**: authentication method and audit log  
- **connectors**: base URLs for dashboard and application  
- **repository**: document storage path  
- **external_configuration**: list of external XML files to be loaded at startup

### Example (excerpt)
```xml
<application>
  <runtime>
    <mode>Devel</mode>
    <repository>E:\$Devel\nexFluxoCore\standard_configuration\</repository>
    <homepath>E:\$Devel\nexFluxoCore\WebApp</homepath>
    <grids><maxRows>20</maxRows></grids>
    <jobs_enabled>true</jobs_enabled>
    <sync_notification>true</sync_notification>
    <revealencrypt>true</revealencrypt>
    <debug_log_level>1</debug_log_level>
  </runtime>

  <database>
    <type>SQL Server</type>
    <connection>Data Source=localhost,1433;Initial Catalog=nexFluxo;User Id=nexFluxo;Password=nexFluxo;</connection>
  </database>

  <authorization>
    <method>kerberos</method>
  </authorization>

  <connectors>
    <dashboard>http://localhost:3000</dashboard>
    <site>http://localhost:62122</site>
  </connectors>

  <repository>
    <path>E:\$DocRepos</path>
  </repository>

  <external_configuration>
    <conf>cors_policy</conf>
    <conf>smtp</conf>
    <conf>multi_language</conf>
    <conf>ldap_sample</conf>
    <conf>kerbersos_sample</conf>
    <conf>saml2_sample</conf>
    <conf>mfa_sample</conf>
  </external_configuration>
</application>
```

### External file precedence
- External files must be placed in:  **`{homepath}\config_extended\`**  
- If a key exists both in `AppConfig.xml` and in an external file, the value in **`AppConfig.xml` overrides** the external one.

---

## 2) External Configuration Files

The following files are supported out-of-the-box:

### 2.1 CORS Policy
Defines allowed origins (multiple values separated by `;`).  
File: `config_extended/cors_policy.xml`  

### 2.2 SMTP
Mail relay configuration and HTML template.  
File: `config_extended/smtp.xml`  

### 2.3 Multi-Language
Enable multiple languages and set the default one.  
File: `config_extended/multi_language.xml`  

### 2.4 Kerberos
Windows authentication mapping.  
File: `config_extended/kerbersos_sample.xml`  

### 2.5 LDAP
Multiple connectors supported with attribute mapping.  
File: `config_extended/ldap_sample.xml`  

### 2.6 SAML2
Integration with external IdP.  
File: `config_extended/saml2_sample.xml`  

### 2.7 MFA
Google Authenticator 2FA.  
File: `config_extended/mfa_sample.xml`  

---

## 3) Key Parameters in AppConfig.xml

- **Runtime / Mode**: `Production | Test | Devel`  
- **Paths**:  
  - `repository` → customization repository  
  - `homepath` → root installation (used to resolve `config_extended/`)  
  - `reports/logo` → absolute path for reports logo  
- **Grids**: max rows in tables/lists  
- **Modules**: external DLLs to be loaded  
- **Jobs**: `jobs_enabled` flag  
- **Debug**: `sync_notification`, `revealencrypt`, `debug_log_level`  
- **Database**:  
  - `type` → `SQL Server` (recommended) or `SQL light` (demo only)  
  - `connection` → provider connection string  
- **Authorization**:  
  - `method` → `local | kerberos | saml2`  
  - `auditlog` → log events, requests, encrypted flag, file path  
- **Connectors**: base URLs for dashboard and frontend  
- **Repository**: path for documents  
- **External configuration**: list of files to load from `config_extended/`  

---

## 4) Setup Checklist

1. Copy `AppConfig.xml` into the app config folder.  
2. Adjust `<runtime>` paths (`homepath`, `repository`, `reports/logo`).  
3. Configure `<database>` connection.  
4. Select `<authorization>/<method>` (local, kerberos, saml2).  
5. Create the `config_extended` folder under `homepath`.  
6. Add external config files as needed (and declare them under `<external_configuration>`).  
7. Configure CORS if required.  (optional)
8. Configure SMTP for email notifications.  
9. Enable multiple languages if required.  
10. Configure LDAP connectors (optional).  
11. Enable MFA (optional).  
12. Restart the application to apply changes.  

---

## 5) Best Practices

- Always use `mode=Production` in live environments.  
- Use valid certificates (SAML2, LDAP SSL).  
- Configure CORS only for required hosts.  
- Keep master `AppConfig.xml` minimal and delegate environment-specific details to external files.  
- Audit logging should be enabled in production for traceability.  
