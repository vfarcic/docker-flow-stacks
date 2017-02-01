This should always be running on the same machine!

To get the ID, ssh into the cluster via manager-1, and run `docker node ls`

```
export SSL_NODE_ID=<node id>
export DOMAINS=('your-website.com' 'your-subdomain.your-website.com')
export CERTBOT_EMAIL=your@email.com
```