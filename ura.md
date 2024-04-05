# Guide for Waterloo Undergraduates

I'm always interested in working with bright and self-motivated Waterloo undergraduates, either as part of the formal [URA program](https://cs.uwaterloo.ca/current-undergraduate-students/research-opportunities/undergraduate-research-assistantship-ura-program) or informally otherwise.
For me, working with undergraduates is an opportunity to expose them to my research passion and also to recruit future graduate students.

## What's In It For You?

What's in it for you?
You get to work on cool projects.
You get to learn new skills.
You get exposure to cutting-edge research.
Not at the start, of course... we work up to that.

The range of outcomes for undergrads working with me is very wide:
At one end of the spectrum, (in many cases) I never hear from you again after the first week.
At the other end of the spectrum, you love working with me so much that you decide to do your Ph.D. with me.
And everything in between... one common outcome is that you start as an URA and end up doing a thesis Masters with me. 

## Initial Screening

If you'd like to work with me, start by working through the [🧱 Foundations of Retrieval](README.md#-foundations-of-retrieval) onboarding path.
By "working through", I mean follow all the instructions, reproduce the results and send a pull request to add to the reproduction log.
You can send me an email after you've progressed through the entire path.
In you email, mention the phrase "banana odyssey": this lets me know you've actually read this document.

The onboarding exercises are meant to be doable on any reasonably modern laptop, although there are known issues with Windows.
If you're having issues, the [student teaching hosts](https://uwaterloo.ca/computer-science-computing-facility/teaching-hosts) are available for UWaterloo CS students.
See [this issue](https://github.com/castorini/pyserini/issues/1611#issuecomment-1703253700) for additional details.

**Note:** Don't bother emailing me until you've done the steps above; if you do, I'll just refer you to this document.
Think of the above as an "initial screen", which is necessary because I get many requests to work with me and don't have time to respond to them all in a customized manner.
The onboarding path provides an opportunity to see if you're truly interested in my work; by working through (even a part of) the exercises, you might discover that it's not interesting to you...
which is completely fine...
but you would have learned something regardless.

## What's the Deal with All This Reproduction?

I believe strongly in open science.
To the extent that I can (keeping in mind that my students are actually the ones doing the work), I insist that _all_ research is backed by open-source code.
But not just open-source code, _usable_ open-source code.
We (try not to) just throw shit over the wall, but release code that allows anyone to _reproduce_ our work, i.e., you can run our code and get the same results.

Almost always, the first tasks that I ask my URAs to do is to reproduce some existing piece of work.
This serves two purposes:

+ For you, it offers an entry into our research. See if you like it.
+ For my group, it provides an extra pair of eyes and hands to ensure that our results are replicable.

Win-win, right?
Along the way, feel free to improve the documentation, for example, clarify a confusing point.

## Choose Your Path

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I present ugrads who want to work with me two character class options: data wrangler or muppet trainer. Most want to be the latter these days.</p>&mdash; Jimmy Lin (@lintool) <a href="https://twitter.com/lintool/status/1387051481841373201?ref_src=twsrc%5Etfw">April 27, 2021</a></blockquote>

After the initial screen, you can choose from three character classes (the two mentioned above, and another one added later):

+ **Muppet trainer.** You want to train neural networks, and in this specific context, you want to work on neural text ranking, QA, etc. If this is you, then take a look at [PyGaggle](http://pygaggle.ai/) as the next step.
+ **Data wrangler.** You want to work on systems-y and infrastructure related issues, e.g., scaling search systems (including neural inference) and munging large datasets with Spark. If this is you, then take a look at [Solr](https://github.com/castorini/anserini/blob/master/docs/solrini.md) and [Elasticsearch](https://github.com/castorini/anserini/blob/master/docs/elastirini.md) integrations with Anserini.
+ **Blocks stacker.** You want to work on end-to-end applications and interfaces, e.g., composing different building blocks into usable software like our [Covidex](http://covidex.ai/) project.

You can get a sense of what each of my research projects are up to by looking at the recent commits, issues, etc.
After the initial screen, here's what the option space looks like:

### Who do I work with and what do I work on?

There are two common paths: sometimes, I'll pass you off to graduate students who need help with something they're working on, and they can serve as mentors.
Sometimes, I'll work with you directly myself.

Either way, our job is to find you tasks that:

+ are concrete and can be clearly described;
+ have clearly-defined success criteria; and,
+ provide learning opportunities for you.

Of course, you can split your time to do both, but these are the tradeoffs:

Working with my students, you'll likely get more "exciting" projects, but they are often more nebuous.

Working directly with me, you'll likely get more "boring" tasks, because I spend a lot of time working on software tooling with the group.


### Critical Path?

One question you should answer is whether you'd like to be on the critical path for our research or work on a side project.
The critical path refers to things that are blockers for our research, for example, that are needed for the next paper deadline.
A side project might be a new avenue of exploration that on one is actively working on.
Both have their advantages and disadvantages.

Being on the critical path means greater time commitment, and that we'll be after you to deliver.
But this likely means you'll get more attention and engagement from me and my graduate students.
A side project is more likely more open-ended, but you'll likely get less focused direction and mentoring.
For these, you'll really have to take the initiative.

### Formal or Informal?

Another issue to think about is whether you wish to do a [_formal_ URA](https://cs.uwaterloo.ca/current-undergraduate-students/research-opportunities/undergraduate-research-assistantship-ura-program).
A formal URA gets paid and I expect commitment on the order of ten hours a week.
I would consider it irresponsible if, halfway through the semester, you simply disappear.
So if you're already taking six courses, a formal URA is probably not for you.
In fact, most students work with me informally, and come and go as they please.

A related question is "What can I put on my resume?"
Don't put URA unless you do a formal URA.
However, I'm fine with more general descriptions such as "research assistant with Prof. Jimmy Lin".
But then you ask, "What's the threshold of my involvement in order for me to put this on my resume?"
That's your call.
I'm not going to police people's resumes (nor do I have time to).
However, keep in mind that everything on your resume is subject to verification, so if an interviewer asks, "Oh, you worked with Prof. Lin, what did you work on?"
You'd better have a good answer.
(It's a small world, and there's a chance that the interviewer knows me personally, or is a former student, etc.)
If the interviewer reaches out to me to check (this has happened before), I should have something meaningful to say.

## My Expectations

Students often ask what my expectations are: the answer is, I have _none_ (unless you sign up for a formal URA).
Yes, seriously.
If I don't hear from you after the first week, I won't think any less of you.
Life is short, everyone has conflicting demands on their time, and you should only work on something if you're interested in it.

My advice is simple: How much you get out of working with our group is directly dependent on how much work you put in.
The single most important trait in successfully working with me, I believe, is being _self-motivated_; generally, technical background isn't an issue &mdash; you got into Computer Science at Waterloo, after all.
With me, there are no deadlines.
How much interaction you get with me and my graduate students depends on how fast you _choose_ to work.
Starting with the "Initial Screen", once you finish something, I'll (try to) give you something else to work on.
If you finish it in a day, I'll give you something else to work on, and the same after that.
Those things will increase in complexity and provide you bigger learning opportunities... until you either stop working (had enough), or you decide you like them so much you want a greater level of commitment (e.g., you want to do a Masters with me).

Note that, at least in the beginning, it may not be uncommon for me to assign the same task to multiple students.
Because of the voluntary nature of URAs, it is hard for me gauge a student's level of interest, or when (if ever) something will be completed.
Typically, I'll try to coordinate and reduce duplication on GitHub issues, but as tasks move on and off the critical path to my other projects, preemption is a possibility.

## DL-fu and Muppet Trainer Character Development

Many students want to become muppet trainers, and I have a relatively well-developed character development progression path.
Your mastery of deep learning skills progresses roughly along the following levels:

+ Level 1: You're able to run inference with an existing model.
+ Level 2: You're able to train a NN from scratch and get reasonable results (with existing code).
+ Level 3: You can make simple tweaks to an existing model.
+ Level 4: You can code up novel NN architectures.

The tasks I assign to URAs also tend to follow along this progression.
Levels 1 and 2 are mostly focused on replication, although replicating training runs is often more complex than just replicating inference runs.
The beginning of level 3 is typically asking you to help us with experiments by making simple parametric changes, e.g., hyperparameter sweeps.
This is usually followed by relatively simple model tweaks (e.g., change max to mean pooling), then followed by increasingly substantive model changes until level 4 is reached.

## Transitioning from a URA to a Grad Student

In a way, being a good URA is easy: it's all about implementation chops.
You get assigned an issue, you close the issue.
Rinse. Repeat.

As a grad student, you need to decide what issues are worth tackling.
In other words, answering a research question is often straightforward.
It's formulating an interesting research question, or deciding _which_ research question is interesting, that's the challenge.
