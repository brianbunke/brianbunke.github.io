---
layout:     post
title:      An IT Ops Manifesto for 2017
date:       2017-07-23 12:00:00 -0700
summary:    Help improve my belief system
categories: blog
thumbnail:  book
author:     brianbunke
tags:
 - manifesto
 - itops
 - manager
 - documentation
---

This is my 2017 manifesto on how to run sustainable IT operations. (I date my manifestos for the same reason I have no tattoos; I don't expect to fully agree with my future self.)

I'm writing it for the same reason I write just about anything:

- I read [something sad], and then prioritized an idea that was on the back burner
- If I preach it internally, I should have enough conviction to make it public record
- I didn't always have all these opinions, so maybe some will be new to you, too
- I hope that someone out there smarter than me (read: you) reaches out to evolve my thinking on a point I hadn't fully considered

As a previous IT manager and current IT team lead: I have not always held all of these convictions, but it's my current belief system, and it's what I currently expect from bosses, as well.

---

## On the agenda:
{: .no_toc}

- ToC
{:toc}

---

## Two modes of IT: Maintain & Improve

Your typical IT department is expected to fix things. They're also expected to keep up with current standards, or more simply, to make things better. Smart organizations value the improvements IT can bring, and less enlightened ones expect IT to just be fast firefighters.

All companies truly expect both of these functions from IT, whether they realize it or not. Are you still running a token ring network?

### - Maintain

Maintaining things is pretty self-explanatory. Users call when things break, or disks fill up. Queueing up maintenance work is also very obvious; someone or something is actively tapping you on the shoulder. Because of that, it's easy to allow maintenance to take more of your time than it deserves.

### - Improve

You then lose that time for improving your environment. It slips very easily: It's working fine right now, so we'll get to it later.

- Refresh the company's oldest workstations
- Upgrade/migrate that server to the new OS
- Replace that firewall with a more modern unit
- Identify and automate standard procedures

I subscribe to the gospel that all **"improve"** operations can (and should) minimize long-term **"maintain"** efforts. _Reducing helpdesk ticket quantity_ is one of the few metrics our industry can generally* agree upon. Don't forget it when you consider the ROI of potential projects.

_\* - Yeah, generally. It's also a potential indicator of [Shadow IT]...which, unfortunately, is a topic for another day._

## IT is a force multiplier

For all my friends currently in situations like ones I've been fortunate enough to leave behind:

It's personally very important to me that I work for a company where IT's value as a [force multiplier] is recognized, instead of being written off solely as a cost center. If IT is just a money pit, all sorts of things become a slog, and you're stuck maintaining old crap forever because no value = no budget.

You can demonstrate value by focusing on projects that affect the bottom line, and putting in the effort to deliver. But sometimes leaders have no intentions of recognizing that value, and you have to cut your losses and move on. _C'est la vie._

## Top-heavy dept

For the past few years, I've favored a "top-heavy," project-focused team whereever possible. My current team is 5 "admins" and 3 helpdesk members, all of varying skill levels.

I like this ratio because of my belief that _improving_ should mitigate _maintaining._

Now, this doesn't mean that your team is 3 maintainers and 5 improvers; far from it. In this example, admins absolutely are still responsible for some maintenance work. Juniors will act in a Tier 2 capacity and bear the majority of it, but seniors should not be fully isolated, either.

Don't let anyone get completely out of touch with the real world.

Following the "out of touch" theme, I've even toyed with the idea of having the team of 5 admins rotate through helpdesk once a week. Ticket volume hasn't necessitated pulling the trigger on that, but I know that's a somewhat controversial idea, and I'm always interested to hear others' thoughts.

## Documentation done right

#### 1. Use a wiki

Remember how you did that infrequent task last year. Stop interrupting others so constantly to ask questions. Plan ahead for vacations, absences, and staff turnover.

I use [Confluence], but you have plenty of options. A built-in revision history of wiki pages is pretty handy. A [WYSIWYG] editor that allows copy + pasting images is also quite helpful.
    
#### 2. Document most things, but not everything

Anything you expect to do in the future should be documented in a wiki page. You'll forget, or you'll quit, or you'll just want someone else to do it. A second person _should_ perform the task, by following the doc, to fill in any gaps.

If it's a one-off bug, just document steps taken in the ticket, and skip the new page. Otherwise, your content is almost guaranteed to go stale and add clutter.

Ask yourself: "When will I/we search for this in the future?"

#### 3. Define your audience

> Write every single doc like a college grad is reading it during his/her first day on the job.

Having a concrete rule like this removes a barrier to _just start writing._ You can adjust for your company, or decide a category of docs can be an exception. But I find that having the guardrail is nice.

#### 4. Empower all team members

The universal #1 complaint about documentation is how quickly the info becomes stale. Wikis don't magically solve that problem, but you can manage it with a combination of wiki features (centralized, indexed searching!) and modifying team behavior.

Wikis only work if _everyone_ is comfortable updating _anything_ that's wrong/missing.

New employees are a godsend here; fresh eyes are a scarce, valuable resource for improving doc quality. Make sure they know from Day 0 that they are empowered to add or clarify _any_ info on _any_ page.

#### 5. Drive wiki adoption

In my experience, you can drill this point home during normal, day-to-day questions.

"Did you check the wiki?" / "I didn't find it in the wiki."

In the beginning, you'll know the answer, but don't be obnoxious about it. If you're doing it right, after a while, you'll have enough pages that the question will be genuine.

If everyone is empowered, and you make the wiki a hub for your daily operations, you're well on your way to attacking the stale info problem.

#### 6. Automate anywhere possible

If you have a list of servers that changes infrequently, you don't want to remember to update the wiki when a server is added, removed, or its configuration changes. Consider scripting a periodic scan, and publishing a wiki update when changes have occurred.

## Alerting and fatigue

I admit to not having read the [Google SRE book] yet, but I love [My Philosophy on Alerting] from Google SRE Rob Ewaschuk, which became a chapter in the book.

If you receive an alert on your phone over the weekend, it should be because a production system is down, the company is potentially losing money, and you personally are needed ASAP. Any alert that is not mission critical should be queued up for when you sit down at your desk with coffee Monday morning.

While you're not at work: Zero push notifications from any automated system trying to be helpful. If you ignore this rule, you will train yourself to ignore them. Superfluous alerts should be terminated with extreme prejudice.

## Evolution of helpdesk

Pokémon need XP to evolve. Your front line friends do, too.

I think I believe three things about helpdesk:

1. Reducing helpdesk ticket quantity is a worthy goal
    - Think about anywhere you can enable end user self-service
2. Rotate tickets through the team
    - No one person is "the laptop dude." More laptop tickets than others, sure. You can still play to your team's strengths
    - Ensure that ticket rotation and documentation will bridge any gaps when "laptop dude" goes on vacation
3. Employees working the helpdesk should get some amount of project time each week

Introducing project work to Jane Doe on the helpdesk has a ton of benefits:

- Breaks up monotony
- Expands Jane's technical and collaboration skills
- Broadens your company's [SME] base
- Paves a path for future promotion

Installing a pipeline to carry new hires beyond their entry-level position (and then backfilling with new entry-level personnel) is great for team morale. Guarantees can't be made, obviously, since you don't have unlimited positions to promote everybody forever.

But with enough correlation of positive/desired traits to the promotion, some potentially great outcomes are available.

## The hiring process

I haven't been great at this in the past, and I haven't done it enough lately to have any hot takes.

Hiring is hard. Like everything else, focus on improving the process instead of celebrating the outcome. One good hire doesn't guarantee the next.

I'll say this: I don't want the interview process to be an interview. I'm passionate about my niches in tech, and I want to talk about what you love about tech. You can't learn everything about a person in an hour, but you can learn a hell of a lot more if you have a conversation, as opposed to a formal interview with obscure gotcha questions.

Identify a drive to learn and a good fit with your team. Writing skill is important, but not always a necessity. And listen to your gut. :)

## Team diversity

Diversity is still a dilemma in 2017, and I have no insightful answers. The tech industry should be more representative of the general population, and we've just started the marathon.

You will not fix this single-handedly, but your awareness helps. Aggressively pursue a welcoming and non-judgmental environment. Treat people equally, challenge your assumptions, and keep your eyes open.

If you have any suggestions, believe me, I'm all ears!

## Certifications and Training

Let's get the sound byte out of the way:

> What if we train them and they leave?
>
> What if we don’t, and they stay?

### - On-demand training

Figure out how your team likes to learn. Try to accommodate that with training that is available to them when they're ready to consume it. We use [Pluralsight], but you won't be starved of options here.

People won't use this as much as you'd expect, and I think that's ok. For example, I use my subscription sparingly right now, but I'm constantly spending time learning outside of work. I usually want to spend that time coding, instead of watching videos.

However, when I want it, it's there. It's a nice luxury to pull up Pluralsight and have a video going in a minute or two. Especially at four in the morning, when asking your newborn for permission to go back to sleep fails. :)

### - Conferences

Try to allocate budget for team members (or at least seniors) to go to a conference or two every year. Meeting colleagues, asking questions about what you're going through, being informed on the `$NewHotness`, and just feeling valued enough by the company to have them foot the bill are all important.

If someone goes to a conference, ask them to talk for 10-15 minutes about their main takeaways. It doesn't have to be a formal report, but at least show interest in what happened there, and ask the employee to be accountable for bringing back a potential recommendation or two.

### - Certification reimbursement

Some people would rather spend conference budget on other forms of training, maybe a boot camp, or a required class for a certification. Go for it!

If anyone on your team passes a certification exam, it's the absolute least you can do to reimburse them the cost of the exam. I mean this: If you can't spare some on-the-clock hours for studying, or potentially a mentor to help spur the efforts, _at least_ pay for the stupid exam.

Some managers have been burned by an employee leaving shortly after receiving training. I haven't been (so, grain of salt), and don't really like the "employee paying back the cert reimbursement if they leave within a year" policy. Why give your team any reason to not pursue additional learning?

Besides, if they leave:

## Turnover, in moderation, is healthy

The goal should not be to assemble the best team and retain them forever. If it happens, great! But it's unrealistic, and likely to get quite expensive. Besides, an outsized focus on retention efforts often is just masking your team's training deficiencies.

Start with Richard Branson:  _"Train people well enough so they can leave, treat them well enough so they don't want to."_

Encourage teaching and learning, embrace a variety of projects to keep things fresh, and quickly address any sources of strife within the team.

Now, consider your pitch to each employee. If your environment follows the rest of this manifesto, you should be well on your way to a healthy and rewarding workplace. Now, here's the main point:

> Don't offer job security, offer _career_ security.

If you have an employee busting ass for you, make sure they're gaining experience that will look good on a resume. And don't just do it silently; let them know that it's important to maintain this long-term focus.

When they finally decide to use that resume, you should feel like a proud parent, helping your kid move to college.

Your workplace probably doesn't have the most money, or the newest tech, or the perfect location. **You will lose people, and that's a good thing.**

Again, if you're following the manifesto, nobody is irreplaceable. The so-called "rockstar" is a failure in the system. Build your team for N+1 redundancy.

Ultimately, this bodes well for future recruitment. If you treat employees well, and then they leave, won't they say good things about you when you're not around?

Focus on employee development. Everything else will fall into place.

## Until 2018

Change is driven most easily from the top down. (Just ask [Satya].) But that doesn't mean you're helpless because you have a boss. Embrace the idea of continuous improvement, and leave it better than you found it.

Remember:

- Evangelize the "improve" function of IT ops
- Be smarter about docs and alerts
- Focus on your employee development pipeline
- When someone moves on, congratulate them earnestly

Be the change you want to see in the world.

---

#### Acknowledgments
{: .no_toc}

- Thanks to [@GlennSarti] for the Shadow IT caveat


[something sad]: https://www.reddit.com/r/sysadmin/comments/6os44g/we_are_canceling_all_training_and_certification/

[Shadow IT]: https://en.wikipedia.org/wiki/Shadow_IT

[force multiplier]: https://personalmba.com/force-multiplier/

[Confluence]: https://www.atlassian.com/software/confluence
[WYSIWYG]: https://en.wikipedia.org/wiki/WYSIWYG

[Google SRE book]: https://landing.google.com/sre/book.html
[My Philosophy on Alerting]: https://docs.google.com/document/d/199PqyG3UsyXlwieHaqbGiWVa8eMWi8zzAn0YfcApr8Q/edit

[SME]: https://en.wikipedia.org/wiki/Subject-matter_expert

[Pluralsight]: https://www.pluralsight.com/

[Satya]: http://fortune.com/satya-nadella-microsoft-ceo/

[@GlennSarti]: https://twitter.com/GlennSarti
