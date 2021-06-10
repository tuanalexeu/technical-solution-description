<h1 align="center">
<br><img src="https://timeweb.com/ru/community/article/1a/1a2d71309f93cef9b598902723e531ca.png" alt="MkDocs icon" width="170">
<br>Domain Name System to make the platform available on the Internet
</h1>

## Description

So that you can access my platform, I bought a Domain Name and pointed it to my ingress external endpoint (static IP address).

<!-- https://shields.io/ -->


## DNS Provider
I chose <a href="https://www.name.com/">name.com</a> as a DNS Provider and here's why:
<dl>
<li>It's much cheaper than any other providers (e.g. Domain.com)</li>
<li>Changing DNS records (A, CNAME) takes a few minutes instead of a couple of 
days (I literally had to wait a day until I could access my site by name after 
changing DNS records while using Domain.com)</li>
</dl>

## Ingress controller

I only have 1 domain and about 5 applications (Not including DB and other stuff). This is where Ingress comes into play:
It allows routing incoming traffic to corresponding services using 1 domain name.
<br>
E.g: I have domain ***logiweb.site***. Now I need to create 'A record' 
named *@* or * (Any matching) which will point to Ingress controller public IP address.
Then, I'll need 4 subdomains which point to initial domain, those are called 'CNAME records', and their values are:
<dl>
<li><a href="https://www.name.com/">dashboard.logiweb.site</a></li>
<li><a href="https://www.name.com/">client.logiweb.site</a></li>
<li><a href="https://www.name.com/">chat.logiweb.site</a></li>
<li><a href="https://www.name.com/">tsd.logiweb.site</a></li>
</dl>

Now, in order to filter incoming traffic, I have to run Ingress controller. Ingress config is as follows:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: logiweb-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  rules:
    - host: logiweb.site
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: logiweb
                port:
                  number: 80
    - host: dashboard.logiweb.site
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: dashboard
                port:
                  number: 80
... the same config for the rest of services
```

Finally, when end user goes to ***chat.logiweb.site*** they'll be pointed to chat service. Great!