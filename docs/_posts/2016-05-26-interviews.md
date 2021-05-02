---
layout: post
title: "Technical testers: mistakes to avoid during interview"
date: 2016-05-26 18:00:00 -0000
categories: testing career
---

Last month a work mate from another team was fired. I knew him a bit and I remember he felt frustrated and overloaded. He said they wanted him to do a very technical checks like troubleshooting a complex distributed system that, in his opinion, was rather a developer's task. Curious of other party's opinion, I asked his manager why he fired him. -- He couldn't cope with his tasks. This position required developer's knowledge but we didn't know that when we were interviewing candidates.

This reminded me of how hard it was for my teams to find really good technical testers. We have spent several months interviewing numerous candidates and it was continued process of improving job descriptions and recruitment techniques. Still we were rejecting a lot of candidates.

I started to investigate whether it is only a problem of my company. Through the survey with 25 members of [Polish testing forum](https://www.facebook.com/groups/TestowanieOprogramowania/?fref=ts) I learned this might be a bigger problem for Polish IT job market. Average time to find a technical tester for survey participants was 3.6 months (ranging from 1 to 8 months) and during this time companies interviewed more than 8 candidate on average (ranging from 1 to 30) until the right candidate was found for a given position. This is pretty long recruitment process comparing to the time we usually spent on finding a developer. It significantly impacts the time necessary to build a good QA team.

So we know it is a bigger problem. But why it happens? In economical terms, demand for technical testers is much higher than supply. This answers, however, does not explain why supply is so low.

In this post I will try to answer that question based on the survey results and follow-up discussions with other testers from the community. If you think about a career of a technical tester, I hope you will learn what mistakes you can avoid during an interview and what skills and habits are worth learning. I also hope this post will raise some awareness of what kind of projects a technical tester might be good for and how to interview them. The history I started with suggests that oftentimes teams may not realize what type of a tester they need.

Why candidates were rejected?
-----------------------------

In the survey people involved in recruitment process voted for most common reasons of rejecting candidates for technical testers positions. They could mark more than one reason. They could also add their own reasons. In the table below I list top 10 reasons for rejection together with a number of people who voted for each one. Basically, those reasons can be split into two groups: insufficient skills and bad attitude. I will explain in details what I mean by that and give examples from my and other participants experiences.

| Main reason for rejection | Confirmed |
| :-- | --: |
| Too small programming skills | 17 |
| Does not want to be a tester | 9 |
| No experience with the technology in the new project | 9 |
| Does not know how to design a test cases of sufficient coverage | 9 |
| Does not know an architecture of a system tested in a previous job | 8 |
| Cannot look for the cause of a bug | 6 |
| Does not want to work with a developer in the design of tests | 5 |
| Does not want to look for the cause of a bug | 4 |
| Does not want to work with a developer at reproducing bugs and finding their causes | 3 |
| Little skills to design test cases | 2 |

### No programming skills

Many candidates have missed basic programming skills. During interviews we often task testers with implementing a simple function that requires combining if/then clauses with loops. Surprisingly, many candidates -- who had been scripting Selenium tests in Python, Java and other programming languages -- have failed to implement simple loops. One of the survey participants explained this might be because 50% of candidates who wrote "automation" in their CVs meant test scripts that were written by someone else, while candidates only executed them and analysed their results.

I have also learned that some testers lack clear code programming skills: they write Selenium tests that are hard to review, hard to debug, and hard to repair. Some months ago we have asked a candidate to automate a test scenario for a simple application. However, automated tests contained so many anti-patterns (see my [another post](http://nomoretesting.logdown.com/posts/734213) on that topic) that we had to reject the candidate.

Why those skills are important tome ? A technical tester:

-   will automate tests on different levels and for different application layers, including backend APIs and isolated components and thus she must be able read existing API definitions and components source code,
-   will write libraries to support test automation, where (["Lessons Learned in Software Testing: A Context-Driven Approach"](http://www.amazon.com/Lessons-Learned-Software-Testing-Context-Driven/dp/0471081124), lesson 126.):

> "Useful libraries require stronger design principles than just avoiding the repetition of code.[...] We have reviewed test suites built with hodge-podge libraries on several occasions. The results are never pretty."

### I don't want to be a tester!

I remember well an interview with one very honest candidate:\
--- Do you have any questions to us?\
--- Yes. Is there a better job here?\
--- Better than...?\
--- Better than tester.\
--- For instance?\
--- Manager's job. I would like to become a test manager in a year.

This candidate hasn't demonstrated great technical skills and had unrealistic expectations. It seemed to us that he did not want to be a tester and considered this profession as worse than that of a programmer or a manager.

In fact, the second most common reason to reject candidates by survey participants was when a candidate did not really want to be a tester. It rather appeared that he or she would treat this position as a transition to be a developer or a test manager. While, in principle, switching a role is not a bad thing, a perspective of such switch in close future brings a risk that a candidate will not treat his or her new job seriously and soon the team will need to seek for a new tester.

### That's not my problem!

In Agile teams responsibilities of testers and developers often overlap and same task can be performed by both groups. For instance, isolating a bug reported by a tester is an example of such a task and there are factors that make a tester often a more suitable person to do that. Danny R. Faught in his article ["How to Make your Bugs Lonely: Tips on Bug Isolation"](http://web.archive.org/web/20110716203602/http://www.tejasconsulting.com/papers/bug-isolation-pnsqc-2004.pdf) lists such factors, concluding:

> "In most of the organizations I've observed, I believe the testers should have been doing more bug isolation, especially for severe bugs."

I observed that it is often more effective when a developer, a customer and tester work together to isolate a bug. Hence, during a job interview I play a role of a customer or a developer and ask a tester for help. Many candidates do not know how to look for the cause of a bug, do not know where to look for it and what tools to use to diagnose the problem. Surprinsingly, some testers do not even want to look for the cause of a bug during an interview. Many of them mentioned they had never paired with a developer to reproduce a bug, saying that in their current or previous companies it was a developer's responsability. It might be a case that when you believe that isolating a bug is not your problem, you may also loose opportunity to learn from developers how to do that.

In total, lack of skills or will to look for a bug cause (alone or with a programmer) was mentioned 13 times (=6+4+3) as a reason to reject a candidate.

### Insufficient testing skills

The book ["Cracking the coding interview"](http://www.valleytalk.org/wp-content/uploads/2012/10/CrackCode.pdf) offers the following task to guage whether a tester can test: "We have the following method used in a chess game: `boolean canMoveTo(int x, int y)`, `x` and `y` are the coordinates of the chess board and it returns whether or not the piece can move to that position. Explain how you would test this method."

However, many candidates fails to solve this or similar tasks. They do not know how to design a test cases of sufficient coverage or has little testing skills in general. Suprisingly, many ISTQB-certified testers have this problem despite the fact, that ISTQB Foundation Level covers techniques for covering testing space (Equivalence Partitioning and Boundary Value).

### I don't care how the system works

9 times a reason to reject the candidate was he or she did not know an architecture of a system tested in a previous job. I often ask candidates how the system was built and how they tested it on different levels, because knowing system architecture

-   helps in isolating bugs
-   helps in resolving environmental problems because you know who to go to to resolve problem with a specific component
-   helps in automating tests on different levels
-   suggests you worked closely with other developers
-   helps in discovering integration bugs (you know where they can occur)

Note, I do not claim that a tester should forget about black-box testing and end-user perspective because:

> "if your primary focus is on the source code of tests you can derive from the source code, you will be covering ground the programmer has probably covered already, and with less knowledge of that code than she had. [...] The advantage of black box testing is that you probably think differently than the programmer, and thus, are likely to anticipate risks that the programmer missed."\
> (from ["Lessons Learned in Software Testing: A Context-Driven Approach"](http://www.amazon.com/Lessons-Learned-Software-Testing-Context-Driven/dp/0471081124), lesson 22).

Threats to survey results validatity
------------------------------------

I do not claim the survey to be statistically valid. The results can be biased, because a sample of surveyed subjects may not be representative for the whole job market. However, the output demonostrates that finding a good technical tester is hard *also* for other companies. Also, I have no hard data about time required to find a developer. It is just experience of my team and HR department that usually spends less time to find a backend developer.

Take-away message
-----------------

\
Having read this post you may ask who actually a technical tester is. I have never clarified that term nor have agreed on it with survey participants but I think the best way to describe such testers is by their skills I have listed in the post. They should be good at progamming, like being a tester and commited to that profession. They should also be able to work closely with a developer in test case design, test automation and bug isolation. They should be creative about inventing test cases and willing to understand how a system under tests works.

And why do you think it is hard to find a technical tester? I'd love to hear what you think about this issue. Let me know in the comments.

P.S. In the next post I plan to discuss why candidates for technical testers may not have such skills and why engineers with right skills do not apply for technical tester positions.
