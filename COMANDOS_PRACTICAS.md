# Comandos Esenciales - Prácticas EGC 2025-26

## � Instalación Inicial

```bash
git clone git@github.com:<TU_USUARIO>/uvlhub_practicas.git
cd uvlhub_practicas
```

### MariaDB
```bash
sudo apt install mariadb-server -y
sudo systemctl start mariadb
sudo mysql_secure_installation  # Password: uvlhubdb_root_password
```

```sql
-- Dentro de MySQL (sudo mysql -u root -p)
CREATE DATABASE uvlhubdb;
CREATE DATABASE uvlhubdb_test;
CREATE USER 'uvlhubdb_user'@'localhost' IDENTIFIED BY 'uvlhubdb_password';
GRANT ALL PRIVILEGES ON uvlhubdb.* TO 'uvlhubdb_user'@'localhost';
GRANT ALL PRIVILEGES ON uvlhubdb_test.* TO 'uvlhubdb_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### Python
```bash
cp .env.local.example .env
echo "webhook" > .moduleignore
python3.12 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
pip install -e ./
flask db upgrade
rosemary db:seed
flask run --host=0.0.0.0 --reload --debug
```

---

## 📊 Práctica 1: Codacy

**Secrets GitHub:** `CODACY_PROJECT_TOKEN`

```bash
coverage run -m pytest app/modules/ --ignore-glob='*selenium*'
coverage xml
coverage report
```

---

## 🚀 Práctica 2: Render

**Secrets GitHub:** `RENDER_DEPLOY_HOOK_URL`

**Desplegar con tag:**
```bash
git tag v1.0.0
git push origin v1.0.0
```

---

## 🎯 Git Básico

```bash
git status
git add .
git commit -m "mensaje"
git push origin main
git pull origin main
```

### Ramas
```bash
git checkout -b nueva-rama
git push origin nueva-rama
```

### Tags
```bash
git tag v1.0.0
git push origin v1.0.0
```

### Deshacer Cambios
```bash
# RESET - Mueve el puntero, reescribe historia (usar ANTES de push)
git reset --soft HEAD~1    # Deshace commit, mantiene cambios en stage
git reset --hard HEAD~1    # Deshace commit y BORRA cambios
git push --force           # Fuerza actualización del remoto (cuidado!)

# REVERT - Crea nuevo commit que deshace cambios (usar DESPUÉS de push)
git revert <hash>          # Deshace un commit específico de forma segura
```

---

## ⚡ Testing

```bash
pytest
coverage run -m pytest app/modules/ --ignore-glob='*selenium*'
coverage report
rosemary db:seed
```

---

**Docs:** https://docs.uvlhub.io/
