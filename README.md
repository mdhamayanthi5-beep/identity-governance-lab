# Identity Governance Learning Lab

A comprehensive learning project to understand **identity governance, authentication, and authorization** using open-source tools.

## 📚 Project Overview

This project sets up a complete identity governance environment with:

- **Keycloak**: Open-source Identity and Access Management (IAM) platform
- **FreeIPA**: Free Identity Management platform
- **PostgreSQL**: Database for Keycloak persistence

Learn how modern identity systems work, manage users, and integrate with applications.

---

## 🎯 Learning Goals

- ✅ Understand SSO (Single Sign-On) and OAuth 2.0
- ✅ Learn identity governance workflows
- ✅ Configure authentication and authorization
- ✅ Manage users, roles, and permissions
- ✅ Integrate identity providers with applications
- ✅ Work with Docker and containerization

---

## 🚀 Quick Start

### Prerequisites

- Docker Desktop installed and running
- WSL 2 updated (on Windows)
- Git (optional, for cloning)
- Terminal/Command Prompt

### Step 1: Clone or Create Project

```bash
# Clone the repository
git clone https://github.com/mdhamayanthi5-beep/identity-governance-lab.git
cd identity-governance-lab

# Or manually download docker-compose.yml to your project folder
```

### Step 2: Start Services

```bash
# Start all services in background
docker-compose up -d

# View running containers
docker-compose ps

# View logs
docker-compose logs -f
```

### Step 3: Access Services

| Service | URL | Credentials |
|---------|-----|-------------|\n| **Keycloak** | http://localhost:8080 | admin / admin123 |
| **FreeIPA** | https://localhost/ | admin / adminpassword123 |

> ⚠️ **Note**: FreeIPA may take 2-3 minutes to initialize on first run. Check logs with `docker-compose logs freeipa`

---

## 📖 Service Details

### Keycloak (Port 8080)

**What it does:**
- User authentication and authorization
- OAuth 2.0 and SAML support
- User management and role-based access control (RBAC)
- Session management

**Default Admin Credentials:**
- Username: `admin`
- Password: `admin123`

**First-time Setup:**
1. Log in at http://localhost:8080/admin
2. Create a new Realm (organization)
3. Create Clients (applications)
4. Add Users and assign Roles
5. Configure Identity Providers (like FreeIPA)

### FreeIPA (Port 443)

**What it does:**
- Centralized user and password management
- LDAP directory server
- Kerberos authentication
- DNS server

**Default Admin Credentials:**
- Username: `admin`
- Password: `adminpassword123`

**Access:** https://localhost/ (self-signed certificate warning is normal)

### PostgreSQL (Internal)

**What it does:**
- Stores Keycloak configuration and user data
- Not directly accessible from outside (by design)

**Credentials:**
- User: `keycloak`
- Password: `keycloak123`
- Database: `keycloak`

---

## 🛠️ Common Commands

### View Service Status

```bash
# See all running containers
docker-compose ps

# View logs for all services
docker-compose logs -f

# View logs for specific service
docker-compose logs keycloak
docker-compose logs freeipa
docker-compose logs postgres
```

### Stop and Start

```bash
# Stop all services
docker-compose down

# Stop but keep data
docker-compose stop

# Start again
docker-compose start

# Restart a service
docker-compose restart keycloak
```

### Clean Up

```bash
# Remove containers but keep volumes (data)
docker-compose down

# Remove everything including data
docker-compose down -v
```

---

## 🔗 Integration: Keycloak + FreeIPA

### What We're Learning

Keycloak can use FreeIPA as an identity provider, creating a centralized identity governance solution:

```
Applications
    ↓
Keycloak (OAuth/SAML)
    ↓
FreeIPA (LDAP/Kerberos)
    ↓
User Directory
```

### Setup Steps (Later)

1. Configure FreeIPA users and groups
2. Add FreeIPA as an LDAP provider in Keycloak
3. Map FreeIPA attributes to Keycloak roles
4. Test user login through integrated system

---

## 📚 Learning Resources

### Keycloak

- Official Docs: https://www.keycloak.org/documentation
- Getting Started: https://www.keycloak.org/getting-started/getting-started-docker
- User Guide: https://www.keycloak.org/documentation/latest/server_admin/

### FreeIPA

- Official Docs: https://www.freeipa.org/page/Documentation
- Quick Start: https://www.freeipa.org/page/Quick_Start_Guide
- LDAP Configuration: https://www.freeipa.org/page/LDAP

### Identity Concepts

- OAuth 2.0: https://oauth.net/2/
- SAML: https://en.wikipedia.org/wiki/SAML_2.0
- LDAP: https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol

---

## 🐛 Troubleshooting

### Keycloak Won't Start

```bash
# Check logs
docker-compose logs keycloak

# Restart
docker-compose restart keycloak

# Check if port 8080 is in use
netstat -ano | findstr :8080  # Windows
lsof -i :8080                  # Mac/Linux
```

### FreeIPA Takes Too Long

- FreeIPA can take 2-3 minutes on first run
- Check logs: `docker-compose logs freeipa`
- Don't stop it mid-initialization

### Database Connection Error

```bash
# Restart postgres first
docker-compose restart postgres

# Then restart keycloak
docker-compose restart keycloak
```

### Port Already in Use

Edit `docker-compose.yml` and change ports:
```yaml
ports:
  - "8081:8080"  # Change from 8080 to 8081
```

---

## 📝 Learning Checklist

- [ ] Docker and services are running
- [ ] Can access Keycloak at http://localhost:8080
- [ ] Can access FreeIPA at https://localhost
- [ ] Created a Keycloak realm
- [ ] Created a Keycloak user
- [ ] Assigned roles to user
- [ ] Configured FreeIPA users
- [ ] Integrated FreeIPA with Keycloak
- [ ] Tested user login flow

---

## 📖 Next Steps

1. **Explore Keycloak Admin Console**
   - Create a new realm
   - Add users and roles
   - Configure clients

2. **Set Up FreeIPA Users**
   - Create users in FreeIPA
   - Organize into groups
   - Set permissions

3. **Configure Integration**
   - Add FreeIPA as LDAP provider in Keycloak
   - Test authentication flow

4. **Build a Test App**
   - Create a simple web app
   - Integrate with Keycloak OAuth
   - Test login/logout

---

## 💡 Tips for Learning

- Take notes on what each service does
- Experiment with creating users and roles
- Check logs to understand what's happening
- Try breaking things safely to learn error handling
- Document your findings

---

## ❓ Questions?

Check the official documentation links above or explore the admin consoles to learn by doing!

Happy learning! 🎓
