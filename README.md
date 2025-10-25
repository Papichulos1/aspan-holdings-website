[create_bundle_Version3.sh](https://github.com/user-attachments/files/23141081/create_bundle_Version3.sh)
#!/usr/bin/env bash
set -euo pipefail

ROOT="aspan-holdings-website"
ZIP_NAME="${ROOT}.zip"

echo "Creating project folder structure in ./${ROOT}/ ..."

# Remove any existing folder/zip to avoid accidental overwrite
if [ -e "$ROOT" ]; then
  read -p "Folder ./${ROOT} already exists. Overwrite? [y/N] " yn
  case "$yn" in
    [Yy]* ) rm -rf "$ROOT" "$ZIP_NAME";;
    * ) echo "Aborting."; exit 1;;
  esac
fi

mkdir -p "$ROOT"
mkdir -p "$ROOT/.github/workflows"
mkdir -p "$ROOT/scripts"
mkdir -p "$ROOT/public"

# Write files
cat > "$ROOT/package.json" <<'EOF'
{
  "name": "aspan-holdings-website",
  "version": "1.0.0",
  "description": "Company website and backend for ASPAN HOLDINGS LLP",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js",
    "test": "node scripts/check-db.js"
  },
  "author": "Generated",
  "license": "MIT",
  "dependencies": {
    "body-parser": "^1.20.2",
    "cors": "^2.8.5",
    "dotenv": "^16.3.1",
    "express": "^4.18.2",
    "sqlite3": "^5.1.6"
  },
  "devDependencies": {
    "nodemon": "^3.0.1"
  }
}
EOF

cat > "$ROOT/.gitignore" <<'EOF'
node_modules
data.sqlite
.env
EOF

cat > "$ROOT/.env.example" <<'EOF'
# Copy to .env and set your admin token
PORT=3000
ADMIN_TOKEN=replace_with_a_strong_token_here
EOF

cat > "$ROOT/README.md" <<'EOF'
# ASPAN HOLDINGS LLP - Website (Node + Express + SQLite)

This repository is a simple full-stack site for ASPAN HOLDINGS LLP:
- Public company page (index.html)
- Contact form (stores messages in SQLite)
- Simple admin page to view/edit company profile and view contacts (token-protected)

Seeded company data:
- Name: ASPAN HOLDINGS LLP
- BEAN: 100240003418
- KATO: 151011100
- Legal address: Kazakhstan, Aktobe region, Aktobe city, Astana district, Altyn Orda microdistrict, building 13D, n.p. 8
- Head: MAKHATOV ALMAS TAZHIBAEVICH
- Postal code: 030000

## Requirements
- Node.js 18+ recommended
- npm

## Install & Run (local)
1. Copy files into a project folder.
2. Copy `.env.example` to `.env` and edit ADMIN_TOKEN:
