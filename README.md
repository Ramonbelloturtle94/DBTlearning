# dbt Local Setup (Snowflake + VS Code)

This guide explains how to set up dbt locally using a Python virtual environment and connect safely to Snowflake using a sandbox schema.

---

DBT = Data Build Tool 
- An open source command-line tool to transform data in the warehouse more effectively. 
- It is the 'T' in ELT (Extract, Load, Transaform) and links directly to snowflake 

Prerequisites:
- Download Visual code or similar
- Install python (3.11)

Installation steps:

## 1. Create workspace folder

```bash
mkdir -p ~/dbt_workspace
cd ~/dbt_workspace

This folder will contain: 

dbt_workspace/
│
├── dbt_env/        # Python virtual environment (local only)
├── dbt_learning/   # dbt project
├── README.md
└── .gitignore

2. Create Python virtual environment (Python 3.11)

Use Python 3.11 explicitly (required for modern dbt versions).

```bash
/usr/local/bin/python3 -m venv dbt_env
source dbt_env/bin/activate
python --version

Expected output: Python 3.11.x

Your terminal prompt should now show: '(dbt_env)'

3. Install dbt packages

Install dbt Core + Snowflake adapter:

pip install -U pip
pip install "dbt-core==1.11.5" "dbt-snowflake==1.11.2"

Verify installation:
~/dbt_workspace/dbt_env/bin/dbt --version

Expected: 

Core: 1.11.x
Plugins:
  - snowflake: 1.11.x

4. Create dbt profiles folder (Snowflake connection)

dbt connection settings live outside the project.
Create the folder:
mkdir -p ~/.dbt

Open the profile file: 
code ~/.dbt/profiles.yml

(If the code command isn't available)

	•	Open VS Code
	•	Cmd + Shift + P
	•	Run: Shell Command: Install ‘code’ command in PATH


Example profile: 

dbt_learning:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: YOUR_ACCOUNT
      user: YOUR_USER
      role: YOUR_SANDBOX_ROLE
      warehouse: YOUR_WAREHOUSE
      database: YOUR_SANDBOX_DATABASE
      schema: RAMON_DBT_SANDBOX
      threads: 4
      authenticator: externalbrowser
      client_session_keep_alive: false

5. Create a dbt project 

cd ~/dbt_workspace
~/dbt_workspace/dbt_env/bin/dbt init dbt_learning

When prompted, select profile: 
dbt_learning


6. Activate environment when starting work

Each new terminal session requires reactivating the environment:
cd ~/dbt_workspace
source dbt_env/bin/activate

Your prompt should now show:
(dbt_env)

7. Test connection 

Run: 
~/dbt_workspace/dbt_env/bin/dbt debug

Expected result:
'All checks passed!'

8. Set up Git & Git Hub 

Create .gitignore (important)

cat > .gitignore << 'EOF'
dbt_env/
target/
logs/
dbt_packages/
.DS_Store
*.log
.env
EOF

Initialise git 

git init

Connect existing GitHub repo

git remote add origin https://github.com/Ramonbelloturtle94/DBTlearning.git
git branch -M main

Commit and push

git add .
git commit -m "Initial dbt local setup"
git push -u origin main


Daily Workflow (recommended)

cd ~/dbt_workspace
source dbt_env/bin/activate
cd dbt_learning
dbt debug

optional shortcut (add to ~/.zshrc):

alias dbtwork='cd ~/dbt_workspace && source dbt_env/bin/activate'

Then run: dbtwork

What lives where? 

Stored locally only
	•	~/.dbt/profiles.yml (credentials)
	•	dbt_env/ (Python environment)

Stored in GitHub
	•	dbt models
	•	macros
	•	YAML documentation
	•	README

Quick sanity checks:
python --version
which dbt
dbt --version
git status
