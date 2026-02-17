# dbt Local Setup (Snowflake + VS Code)

This guide explains how to set up dbt locally using a Python virtual environment and connect safely to Snowflake using a sandbox schema.

---

## 1. Create workspace folder

```bash
mkdir -p ~/dbt_workspace
cd ~/dbt_workspace

## 2. Create Python virtual environment (Python 3.11)

Use Python 3.11 explicitly (required for modern dbt versions):

```bash
/usr/local/bin/python3 -m venv dbt_env
source dbt_env/bin/activate
python --version

Expected output: Python 3.11.x

Your terminal prompt should now show: '(dbt_env)'
