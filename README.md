# rapidserv

A micro web server and http library.

Rapidserv is a micro web server that shares with flask some similarities. It as well implements a http library
that permits to perform asynchronous requests.

Rapidserv is non blocking network I/O consequently it can scale a lot of connections and it is ideal for some applications. 
Rapidserv uses jinja2 although it doesn't enforce the usage.

**A basic Web App**

~~~python
from untwisted.plugins.rapidserv import RapidServ, core

app = RapidServ(__file__)

@app.request('GET /')
def send_base(con, request):
    con.add_data('<html> <body> <p> Rapidserv </p> </body> </html>')
    con.done()

if __name__ == '__main__':
    app.bind('0.0.0.0', 80, 50)
    core.gear.mainloop()
~~~

**A simple Http Client**

~~~python
from rapidlib.requests import get, HttpResponseHandle
from untwisted.network import xmap, core

def on_done(con, response):
    print response.headers
    print response.code
    print response.version
    print response.reason 
    print response.fd.read()

if __name__ == '__main__':
    urls = ['www.bol.uol.com.br', 'www.google.com']
    
    for ind in urls:
        con = get(ind, 80, '/')
        xmap(con, HttpResponseHandle.HTTP_RESPONSE, on_done)
    core.gear.mainloop()
~~~
