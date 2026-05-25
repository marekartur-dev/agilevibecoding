# Underestimated and Annoying, that is "The Dirty Dozen" of programmers - Part 1: the problem space

<img src="./../Images/The_Dirty_Dozen.jpeg" alt="The Dirty Dozen of programmers" width="600"  style="float: right; margin-left: 10px;">

Non-IT people often wonder how software developers know how to write a computer program when the phrase is given that a application is needed that will do something. 
And the answer that probably immediately comes to their mind is that since there are hundreds of thousands of different applications, developers probably know how to do it. 
And what do you, the non-IT reader, think, do they know or not?

## 1. I know/I don't know

Well, I will allow myself to classify the answer to this question as an answer from the category of .. annoying. Most often they do not know. And to be more precise, a more precise answer is, 
they know and they do not know. And you, IT friend, do you agree with me? 

It takes a lot of experience and imagination to predict what will be needed, how to break the problem down into prime factors, what functionalities to implement. 
An inexperienced programmer often does not know how to methodically approach writing a program and improvises all the time. Besides, 'joyful creativity' is a characteristic feature not only of beginners. 
Hence so many never-completed projects. And please do not assume that I am criticizing improvisation. No, improvisation is also necessary, but such action has its limits.

## 2. Welded to the chair 

Writing subsequent lines of code is no problem even for a novice programmer. Writing lines of code is no problem even for a novice coder. 
But collecting those lines of code added over days, weeks, or months into a product that materializes the vision of the solution is usually impossible without refactoring the code or changing the architecture.

What can be irritating is the fact that often the comments that we may be approaching the problem incorrectly are ignored. Without starting a discussion, 
a different opinion may be rejected right away because for a colleague the matter is simple and obvious. However, later, as time passes, this simplicity of the code and the final effect are somehow not visible.

IT specialists realize that almost every commercial software solution requires a systematic approach bordering on madness and the ability to maintain sometimes very complex dependencies in one's head. 
And the integration of systems, without which it is impossible to build a software product today, means sitting in one place and racking one's brains over the problem for many hours in a row. 

We know this, but do we understand that we need to be able to wean ourselves off the chair and force ourselves to do some physical exercise, otherwise we will have serious health problems? 
This is probably something we underestimate. Misjudged.

## 3. PDCA

The persistence and persistence needed in software development can be seen as something bordering on mental illness, and this probably contributes to the perception that programming is boring to many. 
But then again, isn't it interesting that even though programmers usually don't know how to write a program at the beginning, these applications get created?

The answer for non-IT people is CI/CD, Continuous Integration/Continuous Deployment and PDCA, or Plan-Do-Check-Action. Simply put: think, do, check, draw conclusions and continue the same from the beginning. 
For those working in IT, a reminder. Remember that there are design patterns, design principles, best practices, etc., as well as anti-patterns.

And why do I mention PDCA when talking about things that annoy? Because too often, the PDCA cycle is forgotten, neglected, or treat unfairly and we start writing code too quickly. 
Then we just as quickly stop at some weird door-breaking or squaring-the-circle problem.

In addition, there is that "creative chaos" that can appear out of nowhere and without knowing when. It is worth realizing that chaos is a phenomenon with positive feedback. 
Simply put, chaos causes even more chaos and as a result, we quickly stop knowing what to stick to and whether what we are doing makes any sense at all.

Hmmm.. on the other hand, a fruitless search for a solution is not something that does not happen to computer scientists. But the lack of results in the search does not mean a blind search for a solution. 
It is simply good to like learning and to be aware that the point is to find out what will be needed to solve a specific problem. Such charm of this job and this fact alone should encourage learning, mobilize hard work, and not irritate. 
After all, as Henry Ford bitterly observed, "more people waste time and energy thinking about problems than actually solving them." 

Misunderstood and underappreciated.

## 4. Lack of involvement, i.e. code formatting

“Always write code as if the guy who is supposed to be dealing with it is an aggressive psychopath who knows where you live” - the vast majority of programmers agree with this famous quote from Martin Golding. 
This shows how much importance programmers attach to their work. But not everyone cares about it. Some people don't mind that paragraphs and spaces between instructions in their code are disorganized, 
or functions are distributed in a file without logical order and composition. Of course, the above does not mean a lack of commitment, but not caring about the cleanliness of the code is a very bad habit. 
What I am mentioning are trifles? Well, I will repeat after Michelangelo Buonarroti. "Do not despise trifles, because on trifles depends perfection, and perfection is no trifle."

What does not bother some people can irritate many, because it makes it difficult to run the application, find errors or perform necessary code refactoring. Explaining in a hurry does not explain anything, 
because even pressing CTRL-K, CTRL-D on the keyboard takes a fraction of a second, or pointing to a function from the menu with the mouse also takes only a little longer.

Formatting and organizing the visual side of the code is certainly one of the underestimated activities in the field of software development. 
And yet, it is enough to look at how text editors for programmers are changing to notice that the producers of development tools perceive the problem differently. 
After all, aesthetic source code does not only increase the attractiveness of a specialized editor. It is primarily faster software development, fewer mistakes, better quality code.

I understand that many programmers do not pay attention to the colours of constant and variable names, classes or nested hinges, but today we work in teams and our code must be easy to understand also for colleagues in the profession. 
So, developer, if you do not need nicely formatted source code for anything, take care of it for others. 

## 5. Naming convention, code comments, ..

The terminology used by programmers for their own needs can also cause concern among other programmers. And it's not that this terminology is often incomprehensible to people outside of IT. 
The compiler will translate any freak, oddity, hug bug term into machine language.

The terminology used by programmers for their own needs can also cause concern among other programmers. And it's not that this terminology is often incomprehensible to people outside of IT. 
The compiler will translate any freak, oddity, hug bug term into machine language.

The point is that the terminology is often internally inconsistent, does not follow software engineering principles, and after reading the explanations provided, you end up with even more questions than answers. 
This is also an element of the programmers' work that is often underestimated by the programmers themselves, which can be seen not only in the source code, but also in readme files, which are either as is or non-existent.

To quote Martin Fowler, “Good programmers are good at writing code that people understand.” Forgetting data structures, basic principles, or design patterns will show up in code reviews, in your daily work, in comments, and in logs. 
After all, your code is your calling card. 

The lack of understanding of how important it is to comment correctly is not only visible in the source code implementing sophisticated logic. It is also visible in... logs. 
Do you think I mean insufficiently precise descriptions or no descriptions at all? That too, but at this point I mean adding more and more entries to the logs in an ill-considered manner. 
If you think I am too detailed, too annoying, then try working with a new 20GB+ log file created every day, in which the vast majority of entries can be ignored. 
Another aspect is whether the reporting of error details in the logs is well-considered, or whether the reporting of exceptions is also governed by chance.

Also it is worth remembering that the 80/20 rule also works in programming, although its author, the Italian economist Vilfredo Pareto, formulated it in 1896. In the IT community, this idea was popularized by Tom Cargill from Bell Labs in 1985, 
with slightly changed proportions. "Writing the first 90% of the application code takes 10% of the work time. Writing the remaining 10% of the code takes the remaining 90% of the work time". 
So, if your colleague quickly wrote the code and declared it done, leaving you to finish the little things, don't rejoice prematurely.

## 6. Untestable code

Every source code needs to be tested before it becomes a ready-to-use application. The problem is that writing code that cannot be tested is not particularly difficult. Oops-a-daisy! You don't even have to put in that any special effort. 
Simply put, writing ready-to-test code, code that does not mask errors, requires more knowledge than many people might think. You'll find out if it's annoying when you will to debug the code. 
Unless you prefer crossing fingers and deploying to production environment. But it's not a good strategy. 

Am I promoting the Test Driven Development, TDD approach here? Of course I am. But I don't necessarily mean Test First Development, TFD. Starting coding by writing TFD tests will work for very well documented event scenarios. 
But that's a narrow slice of a programmer's work. Most often, programming is solving tasks with a large number of unknowns. In this case, TDD, or constantly remembering that what you write will have to be tested immediately, works great. 
In addition, writing tests right after writing the logic allows for immediate discovery of gaps in the code or mistakes in our reasoning.

It's not without reason that Edsger W. Dijkstra mockingly summed up that "if debugging is the process of removing errors from software, then programming must be the process of inserting them". 
And Dennie van Tassel spitefully added, "I finally learned what the term "backward compatibility" means. It means that all the old errors must be preserved.". 
If you, dear Reader, have now recalled the so-called corollary to Pierce's Law, then you have read my thoughts correctly. "If a program was compiled without errors the first time, it will definitely not give good results."

Besides, if not everything is known, if something doesn't work, you can always make friends with a rubber duck. Don't you know the rubber duck method? Let me explain. 

<img src="./../Images/Rubber_duck.png" alt="The rubber duck" width="600"  style="margin: 10px;">

The rubber duck is a symbol and metaphor for thinking out loud. It's a way to catch and analyse errors in the code. The programmer simply puts a rubber duck next to him and when he has a problem with the code,
he starts analysing it line by line, while explaining to the duck what he's doing and what he sees. Of course, the duck can be replaced with any other toy, mascot, real or imaginary object, person, 
drawing or description explaining the intricacies of the solution to the imaginary reader. However, it is the yellow rubber duck that has become an iconic symbol of this method. Infantile? 
Maybe, but maybe try to be surprised by the usefulness of the method when you solve problems.

## 7. Time pressure

The expected fast writing of code is usually pressure from the business or the client. Well, that can happen too and you have to accept that fact.

But rushing cannot be an excuse for not checking the code properly because the person who made them, the programmer, will get the blame for the mistakes anyway. 

So what, if it is irritating, since bosses and clients do not want to wait too long. Yikes! A sensible boss or client will most likely understand the request for additional time if only they see that you do not follow the rule, 
"what you have to do today, do the day after tomorrow and you will have two days off". After all, it is not surprising that everything can take longer than we expect. And it is good to be optimistic at the beginning of work and critical at its end.

## 8. Working with someone else's code and code merging

Working with someone else's code, often with code that no one can ask for details, is unavoidable.

Before you start tearing your hair out, know that there are people who can write a single function for several hundred lines of code. Classes of this length are no exception, too. Likewise, hundreds of files in one folder, 
each over 1000 lines long. If you're now thinking that it might be better to become a database engineer, because the SQL language is simple and the instructions are short, I'll warn you. 
There are also store procedures over 5000 lines of SQL commands, often accompanied by a lot of before and after triggers. And on top of that, strange errors that shouldn't be there. 

If you get angry quickly and abandon your job when you see something like this, when you're impulsive if the work doesn't go according to plan, then think about whether you really want to be a programmer if you are no one yet. 
And now you can tear your hair out.

What's the problem with this code merging, you ask the reader? We have our own code. Our business partner provides the latest version of their code so we merge it with ours. After all, there are programming tools 
that make such an operation and conflict resolution easier. Yeah .. they make it easier, and you have a list of projects for several screens, several frameworks in one solution and several pipelines. 

An often overlooked problem with combining codes is the fact that this activity must be done in a strictly defined order. First the libraries, then the programs that use them. Of course, if A uses B and B uses A then you have a serious problem, my friend. 
You also have a problem if one solution includes projects built on the basis of different frameworks. I also doubt you will be happy with the CI/CD automation and use of pipelines built into such a solution. 
Jeez! But guys, please note one positive feature of code merge. Code merging is a great way to test errors in the architecture of the system being built.

Phew! No wonder that combining codes is not one of the programmer's favourite activities. If you still don't believe me, ask Uncle Google.

## 9. Documentation

You probably didn't expect me, colleague, to forget about documentation. Fortunately, there's probably no dilemma today about whether or not there should be documentation. But whenever I think about documentation, 
I don't know whether to classify the subject as underestimated or annoying. A strange split personality, Túpac Amaru. And to be precise. I mean only and exclusively working documentation, the one that developers work with. 
Program manuals, marketing materials, business information, orders, etc. for the purposes of this article, let's skip it.

If I think about it, the minimum documentation that a programmer has to work with is two documents.

The first one is a list of requirements with attachments. But someone might be surprised at this point and ask what could be underestimated or annoying about the requirements? Well, that there may not be any requirements! 
Business or client very often try to replace documented description of what they would like with undocumented story about their vision, order to write a program, work order etc. Sorry, but this is not what programmer needs. 

Authorized Application Requirement Document, ARD is something without which no programming work should be started. In medium and large companies, bureaucracy must be regulated, so maybe this problem does not concern them. 
But in small companies it is not so obvious. If it is not possible to get such a document, programmers themselves should create it and submit it for approval. 

Someone will think that it is an unnecessary waste of time, paper, energy, because it is better to get to work right away. I do not share this opinion. You will probably be able to find out about it painfully when it comes to accepting the work, 
financial settlement, and acceptance criteria will be 'by word of mouth'. Experienced programmers will probably even do their best to make ARD exist. Another aspect is the quality of the ARD document, but this is beyond the scope of these considerations. 

The second document that a programmer should work with is .. a readme file or its equivalent. Surprised? Would you, reader, choose something else? After all, it is an optional document. But difference is that document should be created by a developer. 
Do you think that if a programmer wants to add something from himself, he can always supplement the ARD and everything necessary will be in one place? I protest. First of all, the ARD is not a working document 
and the programmer should not interfere with its content once it has been approved.

> ARD is a list of what is expected, while the readme file or its equivalent is a description of what was done and how.

Isn't it obvious? A computer program can implement the expected functionality in many ways and it should be documented somewhere. Is the source code itself the best documentation? After all, while coding, 
the developer is too busy to write documentation at the same time? 

Let's not joke, firstly, the source code is pure syntax. However, at this point we are talking about the semantics of the solution. Secondly, I will add to the fables the story that a programmer will write documentation after he finishes coding. 
No way, we should document live. Later we will forget important details, or we will be too busy with the next target task or tired of the work done.

Dear reader, please choose whether you will include the above in the category of things that are underestimated, annoying, or maybe in another category. And remember to be specific and don't count on others to document what you do better then you. 
According to Weiler's Law, only useless programs have full documentation. But that doesn't mean we should give up trying to make it as complete as possible.

And it's also worth mentioning those non-working and outdated tutorials.

## 10. Pair programming & team work

And since we've already come to the work of a programmer, here we have other opportunities to get irritated. The problem is that the work of many people who create new intellectual quality is not easy in itself.

Let's start by looking at the work of many people by looking at the cooperation of two people. In programming, we talk about pair programming and it's probably not something that has made a dizzying career in software engineering. 
Because how? One sits at the keyboard and types what the other tells him? Or maybe two people simultaneously enter their version of code into the same file? Or maybe each works with their own file and then combines their versions? 

Or do we work on a question and answer basis? This may sound reasonable, but only if the person being asked is open to such joint work. If not, asking even a single question will be irritating and you can forget about a constructive answer.

The term pair programming probably sounds attractive, but at the implementation stage, problems rather than spectacular benefits will immediately come to the fore.

It is simply impossible to determine in advance what such 'pair programming' should look like and it is the performers themselves who have to decide among themselves how they work. And here there are more than enough reasons for conflict. 
Character traits such as conceit and haughtiness of working together do not make it easier. And the more people, the more opportunities for misunderstandings. And please remember that we are talking about work that is supposed to create new added value. 

It is no wonder that a team is often just a sum of individuals working in isolation, instead of people closely cooperating with each other. Deliberately disrupting someone's focus is not something that doesn't happen. 
Well, in principle it is a very difficult topic, but also not impossible to master.

When we work alone, then 2 plus 2 is always 4. But when we work together with someone, then 2 plus 2 has a chance to be more than 4. What is above 4 is the added value, created as a result of one person inspiring another. 
To be fair, it should be added that in a team of 2 + 2 there can also be less than 4. This is when the team is not managed properly or there are conflicts between members.

The need for empathy, openness, communication skills, curiosity, willingness to learn or the ability to solve problems. It does not seem that this problem is unnoticed today, but it can certainly be classified as a sensitive topic. 
After all, one person can be a rock star programmer, someone else just a programmer, and someone else like a third type of programmer who did wrong, does wrong, will do wrong because he does not care.

A separate problem is other people making changes to their code, with which your code closely cooperates, but doing so without any notification. It gets interesting if the changes concern a library to which there is no access. 
But even if there is access, what if your code from a few days ago needs to be changed because for someone else what is most important is what he does and he is not interested in common agreements, and you hear that 'your code has errors'?

And at the same time, what is often irritating is that it happens that programmers close themselves off to ideas other than their own. In the meantime, reflection, talking to someone who understands the problem differently 
and proposes a different approach to the solution, can prevent wasting time and willingness. Well... "where everyone thinks alike, most often no one thinks". 

## 11. Software, Hardware, Network, ..

And what is the difference between the above? If you work on a computer as a programmer, you will be able to handle any problem encountered by a member of your family or friends. Fortunately, awareness of differences among people 
who do not work in IT is high today, but the global decline in education levels guarantees that the need to listen to unequivocal opinions about yourself will continue for a long time yet. 

What kind of programmer are you if you can't even fix a DVD or printer. What kind of specialist are you if you don't know why an attachment won't open or why a notebook won't work? What kind of expert are you if you don't have pirated software 
that your circle of friends needs?

Annoying or underrated?

## 12. Unrealistic expectations

First someone promised customers pie in the sky, and then the programmer had to explain that he didn't do it. Have you ever received requests for something trivial, and you immediately knew that someone had come up with something 
that required very advanced knowledge? Mmmm .. Stockmayer's theorem states about this: If a problem seems easy, it is hard.

So, what is realistic? Edward Aloysius Murphy Jr put it impeccably in the set of laws bearing his name.
- If something can go wrong, it will go wrong.
- If something can go wrong in many places, the first failure will occur where it will do the most damage.
- If you have foreseen four possible failures and have protected yourself against them, a fifth one will immediately occur, for which you were not prepared.
- Things left to themselves go from bad to worse.
- If everything seems to be working fine, you have overlooked something.

There are many more of these laws and rules. I will not list them and I hope that this short list will encourage you to also familiarize yourself with what others think on the subject. I assure it's worth it.

## Summary

So what's above is "the not-magnificent not-the-seven" of software development? I won't confirm, I won't deny. Regardless of whether you work in a team of several thousand programmers, or maybe just in a team of a few people, 
or alone, every programmer should be able to notice in advance both what is undervalued and what, if not now, will be frustrating in a moment. This skill will certainly be reflected in the quality of the code you create. 
That's why it's worth identifying such threats, even from time to time, and if possible, counteracting them.

And speaking of threats, there is one more big unknown at the end, AI. Artificial intelligence has long since added itself to the list of tools used by programmers. Well, when considering AI, a whole range of emotions can be activated, 
with hopes and fears probably coming to the fore. IT is not free from them either, although AI is a product of programmers and they should only be happy about their success. But many people do not even notice the problem with AI, 
but many are already worried. Well, I do not have a ready answer to the threats mentioned, so I am only adding AI to the end of our list out of obligation.

Aha .. being a programmer is an object of desire for many. After all, the modern world cannot do without programmers. There is much less information that this job is not for everyone. So if someone who is not a programmer, 
after reading this still wants to become a software developer, then do not say that no one warned you. Larry Niven, an American science fiction writer, once said bitterly. "Some people think they hate computers. 
What they really hate is bad programmers." Well, it is hard to deny that.

And one more thing. Do not take this as anything else other than experience and thinking about stuff that happens in software engineering. I love and enjoy my work and I hope nothing will change that. 
And you just do what you love and you will never have to work.


Regards