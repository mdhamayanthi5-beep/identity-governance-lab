# Detailed Setup Guide

Follow these steps to get your identity governance lab running.

## Step 1: Download Project Files

### Option A: Using Git (Recommended)

```bash
# Clone the repository
git clone https://github.com/mdhamayanthi5-beep/identity-governance-lab.git

# Navigate to project
cd identity-governance-lab
```

### Option B: Manual Download

1. Download `docker-compose.yml` from the repository
2. Save it in a folder on your laptop (e.g., `C:\Users\USER\identity-governance-lab\`)
3. Open Command Prompt/PowerShell in that folder

---

## Step 2: Verify Docker is Running

Before starting services, make sure Docker is ready:

```bash
# Check Docker version
docker --version

# Check if Docker daemon is running
docker ps -a
```

If you get errors, restart Docker Desktop.

---

## Step 3: Start the Services

Navigate to the project folder and run:

```bash
# Start all services (Keycloak, FreeIPA, PostgreSQL)
docker-compose up -d

# Monitor the startup
docker-compose logs -f
```

**What's happening:**
1. Docker pulls the images (first time only, ~2-3 GB)
2. Creates containers for each service
3. Starts PostgreSQL (usually ready in 10 seconds)
4. Starts Keycloak (ready in ~30 seconds)
5. Starts FreeIPA (takes 2-3 minutes on first run)

Watch the logs. You'll see FreeIPA initialization messages. This is normal.

---

## Step 4: Verify Services Are Running

```bash
# Check container status
docker-compose ps
```

Expected output:
```
NAME                 STATUS              PORTS
keycloak             Up 1 minute         0.0.0.0:8080->8080/tcp
postgres_keycloak    Up 2 minutes        5432/tcp
freeipa              Up 30 seconds       0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp
```

---

## Step 5: Access Keycloak

### URL: http://localhost:8080

1. Open your web browser
2. Go to `http://localhost:8080`
3. You'll see the Keycloak welcome page
4. Click **Administration Console** (or go to `http://localhost:8080/admin`)
5. Login with:
   - Username: `admin`
   - Password: `admin123`

**Congratulations!** You're now in Keycloak admin console. Explore and get familiar with it.

---

## Step 6: Access FreeIPA (Optional - Takes Time to Initialize)

### URL: https://localhost

⚠️ **Important Notes:**
- FreeIPA uses HTTPS with a self-signed certificate (security warning is normal)
- Takes 2-3 minutes to fully initialize
- Check logs if it's not responding: `docker-compose logs freeipa`

1. Open your web browser
2. Go to `https://localhost`
3. Accept the security warning
4. Login with:
   - Username: `admin`
   - Password: `adminpassword123`

---

## Step 7: Explore and Learn

### In Keycloak:

1. **Create a Realm** (organization/workspace)
   - Go to **Realms** → **Create Realm**
   - Name it "myrealm"

2. **Create Users**
   - Go to **Users** → **Create new user**
   - Set username and email

3. **Set User Password**
   - Go to **Credentials** tab
   - Set password (check "Temporary" to force change on first login)

4. **Create Roles**
   - Go to **Roles** → **Create role**
   - Create roles like "admin", "user", "manager"

5. **Assign Roles to Users**
   - Go to **Users** → Select user
   - Go to **Role mapping** → **Assign roles**

### In FreeIPA:

1. Once fully initialized, explore:
   - User management
   - Group creation
   - DNS records
   - Kerberos configuration

---

## Stopping Services

When you want to stop:

```bash
# Stop without removing containers (data is preserved)
docker-compose stop

# Later, start again
docker-compose start

# Complete shutdown (but keeps data in volumes)
docker-compose down

# Remove everything including data
docker-compose down -v
```

---

## Troubleshooting During Setup

### Services Won't Start

```bash
# Check detailed logs
docker-compose logs

# Restart Docker
docker-compose down
docker-compose up -d
```

### Keycloak Keeps Restarting

```bash
# Check Keycloak logs
docker-compose logs keycloak

# Usually means PostgreSQL isn't ready - wait 30 seconds and restart
docker-compose restart keycloak
```

### Can't Access Keycloak (Port 8080)

```bash
# Check if port is in use
netstat -ano | findstr :8080  # Windows
lsof -i :8080                  # Mac/Linux

# If in use, edit docker-compose.yml:
# Change "8080:8080" to "8081:8080"
# Then access at http://localhost:8081
```

### FreeIPA Takes Too Long / Won't Initialize

```bash
# Check FreeIPA initialization logs
docker-compose logs freeipa

# Wait at least 3 minutes
# If still not working, restart:
docker-compose restart freeipa

# View logs continuously
docker-compose logs -f freeipa
```

---

## Next: Learning Modules

Once services are running, see **README.md** for:
- Keycloak configuration tutorials
- FreeIPA user setup
- Integration between services
- Test scenarios

---

## Quick Reference

| Service | URL | Port | User | Password |
|---------|-----|------|------|----------|
| Keycloak | http://localhost:8080/admin | 8080 | admin | admin123 |
| FreeIPA | https://localhost | 443 | admin | adminpassword123 |
| PostgreSQL | localhost:5432 | 5432 | keycloak | keycloak123 |

---

## Getting Help

1. Check logs: `docker-compose logs [service-name]`
2. Restart service: `docker-compose restart [service-name]`
3. Review README.md troubleshooting section
4. Check official docs (links in README.md)

Good luck with your learning! 🚀
