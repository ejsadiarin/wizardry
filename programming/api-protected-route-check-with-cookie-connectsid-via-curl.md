---
date: 2025-01-16T1:34
tags:
  - API
  - How-To
---
<!-- 2025-01-16-0134 (January 16, 2025 01:34:07 AM) -->

# Using Cookie session (`connect.sid`) via curl

> [!IMPORTANT]
> usecase: 
> - if using google login (or any OAuth system)
> - needs browser to login to an endpoint (OAuth login redirects to their page)

## Steps

1. login to the route successfully 

2. inspect devtools
- navigate to "Storage" or "Application" tab
- select "Cookies" from the sidebar and choose your application domain
- locate the session cookie (e.g. `connect.sid`) and copy its value

3. using curl, append `Cookie` header with the `connect.sid=your_session_cookie_value`
```bash
curl -H "Cookie: connect.sid=your_session_cookie_value" http://localhost:3000/protected/resource
```
