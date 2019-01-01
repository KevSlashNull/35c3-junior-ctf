# What Are Flags?
I know this title is a bit misleading. This file is plainly about in what form and where flags (variables) appear in `server.py` and `weeterpreter.ts`.

There are two types of flags. `pyserver` flags and `weeterpreter` flags.

## pyserver Flags
The pyserver flags can be found in the `server_flags()` method and as import at line 18 of `server.py`:

```python
from flags import DB_SECRET, DECRYPTED, DEV_NULL, LOCALHOST, LOGGED_IN

# ...

@app.route("/pyserver/flags.py", methods=["GET"])
def server_flags():
    return Response("""
DB_SECRET = "35C3_???"
DECRYPTED = "35C3_???"
DEV_NULL =  "35C3_???"
LOCALHOST = "35C3_???"
LOGGED_IN = "35C3_???"
NOT_IMPLEMENTED = "35C3_???"
    """, mimetype='text/x-python')

```

All these flags relate to a challenge of the CTF. Of these, all except `NOT_IMPLEMENTED` are used somewhere in `server.py`.

## weeterpreter Flags
The weeterpreter flags can be found in the `weeterpreter_flags()` method in `server.py` and as import in `weeterpreter.ts`. Although, in the `weeterpreter.ts`, the flags are not imported explicitly as

```python
@app.route("/weelang/flags.ts", methods=["GET"])
def weeterpreter_flags():
    return Response("""
export const CONVERSION_ERROR = "35C3_???"
export const EQUALITY_ERROR = "35C3_???"
export const LAYERS = "35C3_???"
export const NUMBER_ERROR = "35C3_???"
export const WEE_R_LEET = "35C3_???"
export const WEE_TOKEN = "35C3_???"
    """, mimetype="text/x-typescript")
```

