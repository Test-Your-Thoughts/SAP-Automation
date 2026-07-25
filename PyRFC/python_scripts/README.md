sap_exec_abap_function.py
=========================

Purpose
-------
Command-line utility to execute a single SAP Remote Function Call (RFC)
against an SAP system using the PyRFC library (SAP NetWeaver RFC SDK
Python bindings). It opens a connection, invokes the specified RFC-enabled
function module with JSON-supplied parameters, prints the result as JSON,
and closes the connection.

Useful for:
    - Automating common SAP activities such as certificate renewals, job failure reports, etc...
    - Ad-hoc testing/invocation of SAP function modules from shell scripts, CI pipelines, or cron jobs.
    - Quick integration checks without writing a dedicated ABAP report.
    - Piping SAP RFC output into other JSON-consuming tools (jq, Python, Node.js, etc.).

Requirements
------------
    - Python 3.x
    - pyrfc (https://github.com/SAP/PyRFC) and the SAP NetWeaver RFC SDK installed and configured (SAPNWRFC_HOME / LD_LIBRARY_PATH in linux set).
    - Network access / valid credentials to the target SAP system.

Connection Parameters
----------------------
    --ashost    Application server host name or IP          (required)
    --sysnr     SAP system number, e.g. "00"                (required)
    --client    SAP client (mandant), e.g. "800"            (required)
    --user      SAP username                                (required)
    --passwd    SAP password                                (required)
    --lang      Logon language (default: "EN")              (optional)
    --sysid     SAP System ID, used for logical/msg-server
                connections if applicable                   (optional)

RFC Invocation Parameters
--------------------------
    --function      Name of the RFC-enabled function module to call   (required)
                    e.g. "STFC_CONNECTION"
    --parameters    JSON string of import parameters to pass          (required)
                    to the function module, e.g.
                    '{"REQUTEXT": "Hello SAP"}'

Usage
-----
Example: calling a function with parameters
    python sap_exec_abap_function.py \
        --ashost sapserver.example.com \
        --sysnr 00 \
        --client 800 \
        --user myuser \
        --passwd mypass \
        --function STFC_CONNECTION \
        --parameters '{"REQUTEXT": "Hello SAP"}'

Example: calling a function with no parameters
    python sap_exec_abap_function.py \
        --ashost sapserver.example.com \
        --sysnr 00 \
        --client 800 \
        --user myuser \
        --passwd mypass \
        --function RFC_PING \
        --parameters '{}'

Output
------
On success, prints the RFC result as indented JSON (non-JSON-serializable values, e.g. datetimes, are coerced to strings via `default=str`).

Security Notes
---------------
    - Passing --passwd on the command line exposes the password in shell
      history and process listings (e.g. `ps aux`). Consider sourcing
      credentials from environment variables or a secrets manager and
      wrapping this script accordingly for production use.
    - No input sanitization is performed on --parameters beyond JSON
      validity; ensure the parameters match the target function module's
      expected signature.
