Use one example: a customer support portal for a retail company (employees log tickets, customers check status).

Here is how responsibility shifts by hosting model:

| Layer / Task | On-premises | IaaS | PaaS | SaaS |
|---|---|---|---|---|
| Physical datacenter, power, cooling, hardware | You | Provider | Provider | Provider |
| Network infrastructure & storage hardware | You | Provider | Provider | Provider |
| Virtualization/hypervisor | You | Provider | Provider | Provider |
| Guest OS patching/hardening | You | You | Provider | Provider |
| Runtime/middleware (web server, app server, DB engine config) | You | You | Mostly Provider (you configure app settings) | Provider |
| Application code (portal features, bug fixes) | You | You | You | Usually Provider (you just configure) |
| Data classification, access control, retention policies | You | You | You | You (still your data/governance duty) |
| Identity/account governance (who can access what) | You | You | You | You |

## Same app, 4 deployment choices

1. On-premises: You run everything in your own server room. Maximum control, maximum operational burden.
2. IaaS: You deploy VMs in cloud (for example, virtual machines). Provider handles hardware; you still manage OS, patches, and app stack.
3. PaaS: You deploy your portal code to a managed app platform and managed database. Provider handles OS/runtime/platform patching; you focus on code + data + identity.
4. SaaS: You adopt a ready-made ticketing product. Provider runs the full application; you mainly manage users, data governance, and configuration.

Quick rule of thumb: moving from on-prem -> IaaS -> PaaS -> SaaS means less infrastructure responsibility for you, more managed by provider, but data and access accountability stays with your business.
