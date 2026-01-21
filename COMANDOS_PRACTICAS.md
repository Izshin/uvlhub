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

## 🔀 Práctica 3: Git Avanzado

### Cherry-pick
```bash
git cherry-pick <hash>     # Trae un commit específico de otra rama
```

### Stash (Guardar cambios temporalmente)
```bash
git stash                  # Guarda cambios sin commit
git stash -u               # Incluye archivos untracked
git stash list             # Ver pila de stashes
git stash apply            # Aplica último stash (lo mantiene en pila)
git stash pop              # Aplica y borra de la pila
git stash drop             # Borra último stash
```

### Patches
```bash
# Método 1: git diff + apply (sin metadatos)
git diff > fix.patch              # Exporta cambios sin commit
git apply --stat fix.patch        # Ver qué hace el parche
git apply fix.patch               # Aplica cambios (NO commitea)

# Método 2: git format-patch + am (con metadatos: autor, fecha, mensaje)
git format-patch -1 HEAD          # Exporta último commit completo
git format-patch -2 HEAD          # Exporta últimos 2 commits
git am 0001-nombre.patch          # Aplica Y commitea automáticamente
```

### Conflictos
```bash
git pull --no-rebase origin rama  # Fusión sin rebase
# Editar archivos con conflicto (entre <<<< y >>>>)
git add archivo.txt
git commit -m "fix: Fix conflicts"
```

### Staging Selectivo
```bash
git add -p archivo.txt     # Añade por partes (hunks)
# y = sí, n = no, s = split, e = edit manual
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

## 🧪 Práctica 4: Testing Avanzado

### Tests Unitarios (models/lógica)
```bash
pytest tests/test_unit.py -v
```

### Tests de Integración (API/endpoints)
```bash
pytest tests/test_integration.py -v
```

### Tests de Interfaz (Selenium)
```bash
# Instalar Chrome/Brave en WSL
sudo apt install chromium-browser -y

# Ejecutar tests de UI
pytest -s tests/test_interface.py
```

### Pruebas de Carga (Locust)
```bash
# Iniciar servidor de pruebas
locust -f locustfile.py

# Acceder a interfaz web
# http://localhost:8089
# Host: http://localhost:5000
```

### Coverage Completo
```bash
coverage run -m pytest tests/
coverage report
coverage html  # Genera reporte en htmlcov/
```

---

## ⚡ Testing General

```bash
pytest
coverage run -m pytest app/modules/ --ignore-glob='*selenium*'
coverage report
rosemary db:seed
```

---

**Docs:** https://docs.uvlhub.io/
