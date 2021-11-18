HTTP security headers are a crucial aspect of website security since they protect you against a variety of attacks such as XSS, SQL injection, clickjacking, and more.
When you use your web browser to access a website, your browser sends a request to the web server where the website is housed. HTTP Response Headers are then returned by the web server. Meta data, status error codes, cache rules, and other information can be found in these headers. It also instructs your browser on how to handle the content of your website. Your browser saves information when you use the internet and interact with websites. These headers will aid in the organization of communication and the enhancement of online security. There are six key security headers to be aware of, and we recommend applying them if at all possible.

There are a few, and as the web evolves, more are being added. Each security header serves its own purpose.

- HTTP Strict Transport Security (HSTS)
- Public Key Pinning Extension for HTTP (HPKP)
- X-Frame-Options
- X-XSS-Protection
- X-Content-Type-Options
- Content-Security-Policy
- X-Permitted-Cross-Domain-Policies
- Referrer-Policy
- Expect-CT
- Feature-Policy

## Content Security Policy (CSP)

The Content-Security-Policy header is an improved version of the X-XSS-Protection header and provides an additional layer of security. It is very powerful header aims to prevent XSS and data injection attacks. CSP instruct browser to load allowed content to load on the website. All major browsers currently offer full or partial support for content security policy.

## X-XSS-Protection

X-XSS also known as Cross Site Scripting header is used to defend against Cross-Site Scripting attacks. XSS Filter is enabled by default in modern web browser such as, Chrome, IE, and Safari. This header stops pages from loading when they detect reflected cross-site scripting (XSS) attacks.

You can implement XSS protection using the three options depending on the specific need.

1.  **X-XSS-Protection: 0** : This will disables the filter entirely.
2.  **X-XSS-Protection: 1** : This will enables the filter but only sanitizes potentially malicious scripts.
3.  **X-XSS-Protection: 1; mode=block** : This will enables the filter and completely blocks the page.

## X-Frame-Options

The X-Frame-Options header is used to defend your website from clickjacking attack by disabling iframes on your site. Currently it is supported by all major web browser. With this header, you tell the browser not to embed your web page in frame/iframe.

There are three ways to configure X-Frame-Options:

1.  **DENY :** This will disables iframe features completely.
2.  **SAMEORIGIN :** iframe can be used only by someone on the same origin.
3.  **ALLOW-FROM :** This will allows pages to be put in iframes only from specific URLs.

Before adding the security headers we can see that the server doesn't respont with any security headers on response.

![enter image description here](https://i.imgur.com/OimRd8t.png)

Now Lets add security headers to our basic web page.
We can create a security folder on the nginx directory and keep our security headers on a specific directory and file

create a localhost.security.conf file and put the following headers

```
# security headers
add_header X-XSS-Protection        "1; mode=block" always;
add_header X-Content-Type-Options  "nosniff" always;
add_header Referrer-Policy         "no-referrer-when-downgrade" always;
add_header Content-Security-Policy "default-src 'self' http: https: data: blob: 'unsafe-inline'; frame-ancestors 'self';" always;
add_header Permissions-Policy      "interest-cohort=()" always;

# . files
location ~ /\.(?!well-known) {
    deny all;
}
```

![enter image description here](https://i.imgur.com/qprMC3P.png)

Now lets include the above file in our localhost configuration

![enter image description here](https://i.imgur.com/zIaF4D3.png)

After including the security headers we can once again visit our web page and see the security headers on the Network tab

![enter image description here](https://i.imgur.com/kNjgFlA.png)
