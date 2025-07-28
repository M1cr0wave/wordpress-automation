# WordPress Plugin Security Auditor

A project by **Nikhil Kanade, University of Waterloo**

This project is a collection of scripts designed to download and analyze WordPress plugins for security vulnerabilities. It uses static application security testing (SAST) tools like Semgrep and CodeQL to scan the plugin source code and identify potential security issues.

## Getting Started

### Prerequisites

* An operating system that is Unix-like (such as Ubuntu)
* At least 30 GB of available disk space
* A MySQL database server
* Python
* Patience, as this can take a while to run

### Steps

1.  Set up a MySQL database server.
2.  Clone this repository:

    ```bash
    git clone [https://github.com/your-username/wordpress-plugin-security-auditor](https://github.com/your-username/wordpress-plugin-security-auditor)
    ```

3.  Configure the `config.ini` file with your database credentials:

    ```bash
    cp config.ini.sample config.ini
    nano config.ini
    ```

4.  Install the required Python dependencies and SAST tools:

    ```bash
    pip install -r requirements.txt
    ```

5.  You may need to log out and log back in to ensure that Semgrep and CodeQL are available in your system's PATH.
6.  Set up the database schema manually (you can skip this step if you provide privileged database credentials in the `config.ini` file):

    * Create a new database and execute the SQL commands found in the `create_plugin_data_table` and `create_plugin_results_table` functions in `dbutils.py`.

7.  Run the main script with the `--download`, `--audit`, and `--create-schema` options:

    * It is recommended to run this script in a `tmux` or `screen` session, as it can take a significant amount of time to complete (potentially over 15 hours).
    * By default, the script will run Semgrep with the `p/php` ruleset. You can specify a different ruleset or tool using the `--config` and `--tool` options.
    * For more information on available Semgrep rules, please see the [Semgrep Registry](https://semgrep.dev/p/php).

8.  Triage the output.
9.  Discover vulnerabilities and report them responsibly.

### Example Usage

```bash
$ python3 wordpress-plugin-audit.py -h
usage: wordpress-plugin-audit.py [-h] [--download] [--download-dir DOWNLOAD_DIR] [--audit] [--tool {semgrep,codeql}] [--config CONFIG] [--create-schema] [--clear-results] [--verbose]

A tool to download and audit WordPress plugins for security vulnerabilities.

options:
  -h, --help            show this help message and exit
  --download            Download and extract plugins. If a plugin directory already exists, it will be deleted and re-downloaded.
  --download-dir DOWNLOAD_DIR
                        The directory to save and audit downloaded plugins (default: current directory).
  --audit               Audit downloaded plugins sequentially.
  --tool {semgrep,codeql}
                        The SAST tool to use for auditing (default: semgrep).
  --config CONFIG       The Semgrep configuration or rules to run. (default: p/php)
  --create-schema       Create the database and schema if this flag is set.
  --clear-results       Clear the audit table before running. This is useful for cron jobs where you only care about the latest results.
  --verbose             Print detailed messages.

$ python3 wordpress-plugin-audit.py --download --audit --create-schema --tool codeql