---
layout:     post
title:      Orienteering at Camp Rubrik
date:       2018-05-21 06:00:00 -0800
summary:    Up close with product and engineers
categories: blog
thumbnail:  rubrik
author:     brianbunke
image:      /images/camprubrik.jpg
tags:
 - rubrik
 - api
---

This week I was fortunate enough to attend "Camp Rubrik - Insider's Edition" at Rubrik headquarters in Palo Alto.

[Camp Rubrik] is an opportunity to learn and get hands-on with the [Rubrik] product in a lab environment. This insider event also afforded a small group of 15 of us to interact with co-founder & CEO Bipul Sinha, a panel of six founding engineers, and gain additional insight from technical marketing, product marketing, and product management teams.

It was a useful (and fun!) event, and I think there are some interesting takeaways to keep in mind when evaluating Rubrik in the future.

[![camprubrik](/images/camprubrik.jpg)](/images/camprubrik.jpg)

---

### Making Day 1 easy

Getting up and running with Rubrik was a primary concern for the founders. The Day 1 experience was a primary driver in shipping Rubrik on commodity hardware, instead of as a pure software solution.

Instant restore or live mount directly to the Brik hardware, rather than worrying about compute or network bottlenecks between source and target.

The UI is simple, modern, and the actions call standard API operations on the back end.

### API-first

Every piece of Rubrik is designed around public, Swagger-based REST API interaction as a primary use case. This means that every event can be driven from the code, allowing for greater automation.

If you know me, you know I love this message. Give customers flexibility to come up with new (occasionally marketable) ideas, then implement them on their own time to improve company-specific workflows. It also means that the UI doesn't need to be cluttered with edge-case features, as [Howard paraphrased]:

> If you're not advanced enough to use the API, you aren't advanced enough to turn the nerd knobs we didn't put in the GUI anyway.

With that said, if you don't bring your own answer to "Why should I care about an API?", you probably won't hear anything solid to convince you. Rubrik is still young and maturing, and they'd like more time before they start "product-izing" their API by presenting specific problems it could address.

Say no more:

### Potential CI pipeline target

Because Rubrik runs on hardware that can instantly mount any backup point, and because any action can be invoked via the API, you can start to dream about additional benefits of a potential purchase.

Not only is Rubrik handling backup/restore/replication/archival/etc. operations, but imagine this workflow:

1. Commit code to your project's source control repository
2. Continous integration (CI) pipeline automatically triggers
3. A SQL database's latest backup point is mounted
    - Natively, without the overhead of the entire SQL server VM
4. Personally identifiable information (PII) is scrubbed from the DB
5. Your most recent SQL backup now acts as a sanitized, on-demand, and up-to-date dev environment for regression tests
6. Tear the DB down once your automated test suite completes, or keep it temporarily available for devs to interact with

This isn't explicitly marketed today, because it could be more robust and user-friendly. Rubrik's mindset has been to prioritize features around backup/restore functionality first, while leaving possibilities like these open for future efforts.

The power of published API endpoints is that you don't need to wait for the dev team. If you care about this, you can code similar workflows yourself, today, as you need them.

### Restore to public cloud with automatic conversion

Consider:

- You protect a local [ VMware / HyperV / AHV ] VM, maybe every four hours
    - (Rubrik treats VM backups as a single snapshot chain, thanks to their [Atlas file system])
- For any reason, you decide to restore it to a public cloud

Your restored VM files automatically convert into an AWS or Azure VM file format, and start running immediately on the public resource you specify.

I definitely am not a "lift and shift" advocate, but if you need to quickly move or restore a couple VMs, that simplicity is pretty appealing.

### One Polaris to track it all

Many customers start with one on-prem datacenter. Over time, subsets of data become hosted in other datacenters, or a pilot project moves into AWS/Azure/Google Cloud.

[Polaris] is meant as a "unified system of record." In other words: File-level restores of your backups require a lot of metadata to keep track of everything. Polaris exposes that metadata back to you, and acts as a searchable "metadata brain" that doesn't care if your data sprawl includes many datacenters or multiple public cloud providers.

As workloads continue to migrate around your hybrid infrastructure, Polaris is positioned to watch it all for you. There are _a lot_ of potential applications here outside of the typical backup-related workflows.

### What's the catch?

I think Rubrik is pretty cool technology, with plenty of room to grow. They do, too, and it's priced at a premium.

I work at a SMB customer, and the cost is a steep ask at our smaller environment scale. You may want to be north of 500 VMs before you engage sales. Obviously, YMMV, but Rubrik isn't going to drop into every IT shop overnight.

If your infrastructure is larger, I'd encourage you to get up to date with Rubrik's feature set and consider what problems it may solve for you.

---

**P.S. -** Lots of [pics on Twitter] of both work and play. Allow me to enthusiastically recommend the [California Academy of Science] and the 5v5 arcade game [Killer Queen]!



[Camp Rubrik]: https://www.rubrik.com/company/events/camp-rubrik/
[Rubrik]: https://www.rubrik.com/

[Howard paraphrased]: https://twitter.com/DeepStorageNet/status/997167056565907457

[Atlas file system]: https://www.rubrik.com/blog/introducing-atlas-rubriks-cloud-scale-file-system/

[Polaris]: https://www.rubrik.com/product/polaris-overview/

[pics on Twitter]: https://twitter.com/brianbunke/status/997533601376813056
[California Academy of Science]: https://www.calacademy.org/
[Killer Queen]: http://killerqueenarcade.com/
