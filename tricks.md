## command line 用 python

### 列印 json string

```
echo '{"okey1": "value", "okey2": {"ikey": "value"}}' | python -mjson.tool
```

### simple smtp server for debugging

```
sudo python -m smtpd -n -c DebuggingServer localhost:25
```

### The zen of python

```
>>> import this
```
