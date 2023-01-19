---
title: "Event streams querying"
date: 2019-10-11T21:27:04+02:00
draft: false
---
Codes: [GitLab](https://gitlab.com/HawkSVK/event-streams-querying)

As semestral project in my exchange program, me and my colleague created java program for searching in Log files.

These files was few hundred thousand lines long. Log was written in custom language that was simmilar to JSON. So we was forced to write our own parser. That was job of my friend.

My task on this project was to use objects (that he parsed) and search throug them. I used for this Esper language and runtime for event processing. Our customer however wanted to use simple language for searching. So my task was also to create simplified query language that I then translated to Esper.
