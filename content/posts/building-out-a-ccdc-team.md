---
title: "Building Out A CCDC Team"
date: 2019-05-17T09:00:00-07:00

tags: ['CCDC']
author: "shellcromancer"
noSummary: false

resizeImages: false
---

This post is going to be about my experience with the Collegic Cyber Defense Competiton (CCDC) as a general competitor but more as a captain last year.

For those not familar with the CCDC compeition this is an event where a team of 8 students act as the defensive blue team to secure and administer a horribly insecure network. At the same time we start with the defense of this network there is an Agressive and Advanced Persistent Threat (AAPT, coined by [@1njection](https://twitter.com/1njection)).

Last year I acted as my teams captain and through this spent a lot of time writing up documentation, thinking about how the team should be structured, and what kind of things I wanted the team to know. A lot of this was combing through the CCDC write-ups that I could find in online so here are a lot of the resources that talked about this structure:

* http://howtowinccdc.com/
* https://medium.com/@malcomvetter/how-to-win-at-ccdc-b355e309a05c
* https://www.wraysec.com/2016/03/14/how-to-win-the-ccdc/

Based off of reading these I decided our teams structure would be as following:

1. Captain
2. Injects Lead
3. Networks Lead
4. Linux Lead
5. Linux Help
6. Web Lead
7. Windows Lead
8. Windows Help


While we broke the team out into the subgroups to focus, I also wanted to ensure everyone involved on the team was cross-training with the fundamentals for all areas so I led trainings on: computer networking best practices, linux/windows fundamentals, web apps with hardening an Apache/Nginx webserver, fundamentals of incident response, and finally on some of the adversary tactics and toolings that the CCDC red team would be using.

One thing that was important to me is that I kept roster selections fair and open to everyone and I think the best way to hit this point is with [Amanda's post here](https://amandaszampias.blogspot.com/2019/03/ccdc-call-for-merit-and-fairness-in.html). I did this by basing team selection off of success in completing learning assignments for the roles, filling in documentation and finally participation in invitational events.

### ROLE BREAKDOWNS:
#### INJECTS LEAD:
CCDC is more than just a strictly technical compeition... As the team figures out what maddness they've been handed and handles the red team barrage, you still need to report to the business. Injects can be anything ranging from submitting a good password policy, take inventory of what your network has, setup mail accounts & image for new employees or give a presentation on how to mitigate phishing. From my experience these tasks range from 30-40% of your overall score depending on the event and organizers so this role is critical to overall success.

The injects lead is the member who spearheads writing technical briefings and reports to the CEO & other leadership. They should be able to convert that nmap scan into a nicer looking inventory and topology on their own. This means they should have a fair amount of technical chops but also be confident in presenting, and conveying these things to folks who aren't supposed to be technically savvy. Key things to look for in this injects lead is a person who is timely, detail-oriented, and can wing a report on a moments notice. Before the competition they can prepare templates for the most common injects to save time during the event.

#### NETWORKS LEAD:
The networks lead is probably has the most direct impact on the success or failure of the CCDC team. This lead is responsible for quickly applying restrictive ingress/egress filtering, setting up a web/DNS proxies, and putting down some form of IDS. Some mistakes with knowing how to set these things up can bring the whole network down and cause lots of fingers to point your way (or just give red-team a lot more credit than they deserve). Once these are successful, take the time to see what network segmentation you can create to differentiate your team from other compeitors and inspect traffic to see what red team is up to.

One thing to keep in mind: just because you have an expensive appliance like a PAN doesn't mean you're safe: during my first year playing as the networks lead I had a red-teamer mess up our box pretty goodd. He was trying to revert me to a vulnerable version on PAN-OS but I kicked him out in the process so he stuck me into DFU mode. One of our next injects was to roll out support for Wildfire, get AV signatures and all of the valuable parts of the PAN box, and we were out of luck with the Palo Alto support rep standing next to me confused to what the hell went wrong for several hours.... Work on the basics and ensure red team doesn't get access to these network appliances cause they can cause a whole ton of hell once they're in.

#### LINUX & WINDOWS TEAMS:
The 2-person distributions teams are responsible for all of the specifics per their systems. These are the areas where having the scripted solutions done before hand comes in handy and they can spend their time getting a complete inventory, patching holes that scripts don't fix, and all other host related tasks. In the past we've used tools like [OSSEC](https://www.ossec.net) with some success but we rarely have time to actually get a working install. On these endpoint teams you should be familiar with the distributions way of: auditing accounts, auditing processes/services, creating ACL's, and securing common services (like DNS, Mail, FTP, SSH/RDP, etc.).


#### WEB ADMIN:
A lot of teams don't have a dedicated person to focus on securing websites but we decided we wanted this as sometimes we could have up to 10 websites to work on and there is a wide range of ways that it could be messed with. The web admin focuses on adding authentication to web apps that need it, putting mod_security with the [OWASP Core Rule Set](https://modsecurity.org/crs), and sometimes even building out any custom web-apps that need it. We have filled this role with folks that come from developer backgrounds, and are familiar with deploying their applications on servers like Nginx/Apache (and sadly even IIS).

#### CAPTAIN:
Intentionally not many duties are left for the captain during the competition. This job is mostly done before events by picking what technologies might be optimal for the competition, drafting workshops/assignments and whatever organizationally needs to happen for the team to show up and kick ass. My time as the captain was mostly spent doing documentation of our groups runbooks per role as well as general IR plans.
During the event itself, the captain role shifts to decision making on whatever sacrifices that come up, making sure members aren't digging into a rabbit hole for too long on specific tasks, and many other communication oriented tasks. Feel free to help teamates by working on the computers when you can but be cautious of this as you lose sight of the bigger picture with what's going on.

### STRATEGY:
My perspective on how to win is taking the optimization approach to the game:

```
Score = ((Injects) + (Service Uptime) * -SLA’s) - (Attack Deductions)(.5)
```
A lot of folks I talked to suggested the turtle approach where during the first 30-60 mins you shut off all inbound connections while you have time fix the common exploit vectors like shared passwords, vulnerably software, and other misconfigurations; to prevent this from being a viable strategy the gamemakers have introduced higher SLA penalties during the first hour of the compeition.

My goal was to come into the competition with automation scripts (ansible plays) to do most of the inital hardening, and set up the security infrastructure that we would use. Everyone would stay on top of their responsible duties, as we locate and remove vulnerbilites in the network. [Red team is always going to be there](https://twitter.com/1njection/status/1121814976791031808), you just need to find them.

The reason I only factor in 50% of the red team deductions because you can earn these points back with submissions of "Incident Reports". This shows blue-teamers the value of keeping logs on everything, and valuable IR skills. This is a great enviroment to try containing and forcing out an AAPT; much better to learn how to do it here than at your Fortune 500 company. As long as you view the logs enough and they are verbose enough you should see plenty of evidence of red-team.

### RECAP:
Realistically what makes a team succeed is the same as with real life: having the passion to learn all of the topics that the classroom and whatever corporate trainings you get sent to doesn't cover. CCDC gives the hackers hands on defensive skills [like Alex mentions here](https://alexlevinson.wordpress.com/2018/04/21/ccdc-is-the-real-world-and-heres-why/) and more folks should get involved with the CCDC community.

To future compeitors: I hope this helps in some form in planning for an upcomming season, and best of luck! I'm always happy and excited to talk to CCDC folks so reach out via twitter [@shellcromancer](http://twitter.com/shellcromancer)!

My time as a blue-teamer for CCDC has wrapped up so I'm planning on getting involved with the red/black teams to continue giving this awesome oppurtunity to more folks in the future. I hope other past competitors do something similar too. Cheers!
