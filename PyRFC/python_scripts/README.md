# sap_exec_abap_function.py

A command-line utility to execute a single SAP Remote Function Call (RFC) against an SAP system using the [PyRFC](https://github.com/SAP/PyRFC) library (SAP NetWeaver RFC SDK Python bindings). It opens a connection, invokes the specified RFC-enabled function module with JSON-supplied parameters, prints the result as JSON, and closes the connection.

## Use Cases

- Automating common SAP activities such as certificate renewals, job failure reports, etc.
- Ad-hoc testing/invocation of SAP function modules from shell scripts, CI pipelines, or cron jobs.
- Quick integration checks without writing a dedicated ABAP report.
- Piping SAP RFC output into other JSON-consuming tools (`jq`, Python, Node.js, etc.).

## Requirements

- Python 3.x
- [`pyrfc`](https://github.com/SAP/PyRFC) and the SAP NetWeaver RFC SDK installed and configured
  - `SAPNWRFC_HOME` environment variable set
  - `LD_LIBRARY_PATH` (Linux) configured to include the SDK's `lib` directory
- Network access and valid credentials to the target SAP system

### Installing dependencies

```bash
pip install pyrfc
```

> Note: `pyrfc` requires the SAP NetWeaver RFC SDK to be installed separately and is not distributed via PyPI in all environments — refer to SAP's official documentation/SAP Store for the SDK download.

## Installation

```bash
git clone <repo-url>
cd <repo-directory>
pip install -r requirements.txt  # if provided, otherwise: pip install pyrfc
```

## Usage

```bash
python sap_exec_abap_function.py \
    --ashost <host> \
    --sysnr <system-number> \
    --client <client> \
    --user <username> \
    --passwd <password> \
    --function <function-module-name> \
    --parameters '<json-string>'
```

### Arguments

#### Connection Parameters

| Argument    | Required | Description                                                        |
|-------------|----------|---------------------------------------------------------------------|
| `--ashost`  | Yes      | Application server host name or IP                                 |
| `--sysnr`   | Yes      | SAP system number, e.g. `00`                                       |
| `--client`  | Yes      | SAP client (mandant), e.g. `800`                                   |
| `--user`    | Yes      | SAP username                                                       |
| `--passwd`  | Yes      | SAP password                                                       |
| `--lang`    | No       | Logon language (default: `EN`)                                     |
| `--sysid`   | No       | SAP System ID, used for logical/message-server connections if applicable |

#### RFC Invocation Parameters

| Argument       | Required | Description                                                                 |
|----------------|----------|-------------------------------------------------------------------------------|
| `--function`   | Yes      | Name of the RFC-enabled function module to call, e.g. `STFC_CONNECTION`     |
| `--parameters` | Yes      | JSON string of import parameters to pass to the function module            |

### Examples

**Calling a function with parameters:**

```bash
python sap_exec_abap_function.py \
    --ashost sapserver.example.com \
    --sysnr 00 \
    --client 800 \
    --user myuser \
    --passwd mypass \
    --function STFC_CONNECTION \
    --parameters '{"REQUTEXT": "Hello SAP"}'
```

**Calling a function with no parameters:**

```bash
python sap_exec_abap_function.py \
    --ashost sapserver.example.com \
    --sysnr 00 \
    --client 800 \
    --user myuser \
    --passwd mypass \
    --function RFC_PING \
    --parameters '{}'
```

## Output

On success, the script prints the RFC result as indented JSON to `stdout`. Non-JSON-serializable values (e.g. datetimes) are coerced to strings via `default=str`.

```bash
python sap_exec_abap_function.py ... | jq .
```

## Exit Codes

| Code | Meaning                                      |
|------|-----------------------------------------------|
| `0`  | Success                                       |
| `1`  | Invalid JSON passed to `--parameters`         |
| `2`  | Communication error (network/connectivity)    |
| `3`  | Logon error (invalid credentials, locked user, etc.) |
| `4`  | ABAP application or runtime error             |
| `5`  | Unexpected/unhandled error                    |

These exit codes make the script convenient to use in shell scripts, CI pipelines, and cron jobs where error branching is needed.

## Security Notes

- **Passing `--passwd` on the command line exposes the password** in shell history and process listings (e.g. `ps aux`). For production use, consider:
  - Sourcing credentials from environment variables
  - Using a secrets manager (e.g. HashiCorp Vault, AWS Secrets Manager)
  - Wrapping this script in a secure launcher that injects credentials without exposing them as CLI arguments
- **No input sanitization** is performed on `--parameters` beyond JSON validity. Ensure the parameters match the target function module's expected signature before invoking against production systems.

## License

Add your license information here.
