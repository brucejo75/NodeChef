# NodeChef and Custom Domains
The NodeChef documentation is a little spotty on how to point your custom domain to your NodeChef domain with Transport Level Security (TLS).

Essentially, there are 3 parts to the problem:
1. Validate your domain with NodeChef.
2. Point your domain to the NodeChef application.
3. Add SSL.

## 1. Validate your domain with NodeChef
The instructions are [here](https://nodechef.com/docs/node/global/domains).  So essentially, you add a TXT record on your domain
or subdomain.

What the fail to tell you is:  After you have validated the domain.  **Now you must delete the TXT record.**  This is OK,
because NodeChef only needs to validate once, it does not repeatedly validate.

This is important, because my DNS provider (GoDaddy) does not allow you to add a TXT record to a sub-domain that has a
CNAME/A record other than @.  Nor can you add a CNAME record to a domain that has a TXT record.

Why?  Because essentally, adding that TXT record would be interpreted to be you owning the IP/DOMAIN you are pointing to.
An obvious security problem.

## 2. Point your domain to the NodeChef Application
At your DNS, now add a CNAME record that points to your nodeChef application.
But you can only do this __after__ you have deleted the TXT record.

## 3. Add SSL
Now you can follow the directions [here](https://nodechef.com/docs/node/global/letsencryptssl) and add SSL encryption using LetsEncrypt.

## EXAMPLE
1. Enter the name of the domain in Custom domain settings.

Here I submitted `sub.sub.yourdomain.com`:

![alt text](https://github.com/brucejo75/NodeChef/blob/master/Add%20Custom%20Domain%204.png "Add Custom Domain")

2. Add the TXT field to your domain

DNS Management Records for yourdomain.com

| Type | Name | Value |
| ---- | ---- | ----- |
| A | @ | 144.XXX.XX.XX |
| TXT | sub.sub | nodechef=111222aaabbb |

3. Validate the domain

hover over the domain field and click the validate link.  This may not work at first.  The DNS TXT record is busy propagating
throught the network.  It usually ready within 5 min.

4. Remove the TXT field from your domain records & add in the CNAME to the nodechef app.

DNS Management Records for yourdomain.com

| Type | Name | Value |
| ---- | ---- | ----- |
| A | @ | 144.XXX.XX.XX |
| CNAME | sub.sub | yourapp-00.nodechef.com |

5. Add SSL as described [here](https://nodechef.com/docs/node/global/letsencryptssl)

Done!
