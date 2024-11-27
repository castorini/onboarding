# Guide for Waterloo Undergraduates

I'm always interested in working with bright and self-motivated Waterloo undergraduates, either as part of the formal [URA program](https://cs.uwaterloo.ca/current-undergraduate-students/research-opportunities/undergraduate-research-assistantship-ura-program) or informally otherwise.
Working with undergraduates is an opportunity to expose them to my research passion and recruit future graduate students.

## What's In It For You?

What's in it for you?

You get to work on cool projects.
You get to hone existing skills or learn new skills.
You get exposure to cutting-edge research.

If you're considering applying to graduate school beyond Waterloo, I might write you a rec.
If you're considering doing a Master's (or beyond) at Waterloo, you might receive an offer to join my group.
Not at the start, of course; we work up to that.

The range of outcomes for undergrads working with me is extensive:
At one end of the spectrum (in many cases), I never hear from you again after the first week (totally fine!).
At the other end of the spectrum, you love working with me so much that you decide to do your Ph.D. with me (see, for example, [Xueguang Ma](https://mxueguang.github.io/) and [Ronak Pradeep](https://ronakice.github.io/)).
And everything in between... one common outcome is that you start as a URA and end up doing a thesis Master's with me (see, for example, [Michael Tu](https://www.tuzhucheng.com/) and many others).

## Initial Screening

If you'd like to work with me, start by working through the [🧱 Foundations of Retrieval](README.md#-foundations-of-retrieval) onboarding path.
By "working through," I mean following the instructions, reproducing the results, and sending a pull request to add to the reproduction logs.
You can send me an email (with pointers to your pull requests) after you've progressed through the entire path.
In your email, mention the phrase "banana odyssey": this lets me know you've read this guide.

The onboarding exercises should be doable on any reasonably modern laptop, although there are known issues with Windows.
If you're having issues, the [student teaching hosts](https://uwaterloo.ca/computer-science-computing-facility/teaching-hosts) are available for UWaterloo CS students.
For additional details, see [this issue](https://github.com/castorini/pyserini/issues/1611#issuecomment-1703253700) (on dealing with CPU limit) and [this issue](https://github.com/castorini/anserini/pull/2572) (for installing Java 21).

❗ Don't bother emailing me until you've done the steps above; if you do, I'll refer you to this guide.
Think of this as an "initial screen," which is necessary because I get more requests than I have time to respond to in a customized manner.
The onboarding path provides an opportunity to see if you're interested in my work; by working through the exercises, you might discover that it's not interesting to you..., which is fine, but you would have (hopefully) learned something.


## What's the Deal with All This Reproduction?

I believe strongly in open science.
To the extent that I can (keeping in mind that my students are doing most of the work), I insist that _all_ research is backed by open-source code.
But not just open-source code, _usable_ open-source code.
We (try not to) throw shit over the wall but release code that allows anyone to _reproduce_ (_repro_ for short) our work, i.e., you can run our code and get the same results.

Almost always, the first task I ask students (URAs and even new graduate students) is to reproduce some existing chunk of work.
That serves two purposes:

+ For you, the student, it offers an entry into our research. See if you like it.
+ For me and my research group, reproduction efforts provide extra pairs of eyes and hands to ensure our results are reproducible.

Win-win, right?
As you go, feel free to improve the documentation—fix a typo, clarify a confusing point, etc.
Send a pull request.

If you're interested in learning more, read my manifesto ["Building a Culture of Reproducibility in Academic Research"](https://arxiv.org/abs/2212.13534).
As a minor terminological note, ["reproducibility" and "replicability" are _not_ the same](https://github.com/castorini/anserini/blob/master/docs/reproducibility.md).

❗ I've written this elsewhere, but it's worth repeating: when doing the repros, try to understand what you're doing instead of simply [cargo culting](https://en.wikipedia.org/wiki/Cargo_cult_programming) (i.e., copying and pasting commands into a shell).
Yes, you quickly go through the guides if you do that.
For that matter, we could write our repro guides as notebooks, and you can do "Run all" to repro... but that defeats the point (which is why we _don't_ create notebooks).
Use the repro guides as springboards for additional exploration.
_Be curious._
Say to yourself: Wait, this step seems to hide complexities.
I wonder what's happening behind the scenes?
Why don't I go off on a "side quest" to explore?
Side quests are encouraged!

## What Happens Next?

After you've finished the onboarding, I'll send you a Slack invite and drop you into a channel where all the URAs "hang out."
It's a channel for high-level coordination.
From there, DM groups and other channels get formed around specific tasks and projects.
Sometimes, this channel is full of activity (particularly at the start of semesters); other times, it's quiet.

Start by scrolling around and reading recent activity in the channel to see what everyone's been up to.
We keep a [backlog](https://github.com/castorini/ura-projects/issues) of things for URAs to work on. It's worth browsing through, but may not be up to date.

From here, your natural question is: Once I've onboarded, what do I do?

I'll answer that question with another question.
I often give the following advice, which is applicable here: start at the end and work backward.

What do you hope to get out of the URA?

Possible answers include:

+ I want to learn what "research is all about."
+ I want to learn new skills.
+ I want to better prepare for graduate studies.
+ I want a rec for graduate school.
+ I want to contribute to a paper and hopefully be included as a co-author.
+ I want to join Jimmy Lin's research group as a graduate student.

So, start with your goal (or goals) in mind, work backward, and "do" whatever best aligns.

At a high level, your three options are:

1. Work on a task of mine.
2. Help out one of my graduate students.
3. Wow us.

Of course, you can do a mix of all three.

### Work on a Task of Mine

I usually have a backlog of tasks I'd like to get done.
If you work directly with me, you'll likely given a task more focused on software engineering.
My hacking focuses more on tooling, UIs, and improving the experience in our Anserini/Pyserini toolkits.
You might even call these tasks "boring."

However, you get direct interactions with me, and my tasks allow you to make observable, concrete progress.
In other words, you'll get "quick wins."
If you're looking for a rec for grad school, it has to come from me, so giving me a chance to know you isn't a bad idea.
Of course, this is a double-edged sword: If you're competent and impress me, I can help you accomplish your goals.
But on the other hand, I can also see weaknesses and gaps.

Working on my tasks (and completing them promptly) builds credibility with me and often "unlocks" more rewarding research projects and multi-way interactions involving my graduate students.

Note that it is _not_ the case that these tasks, despite their focus on software engineering in many cases, preclude publications.
For example, this demo paper ["Vector Search with OpenAI Embeddings: Lucene Is All You Need"](https://dl.acm.org/doi/10.1145/3616855.3635691) started from just "hacking around" with different vector search implementations I had in mind.
Here's an arXiv paper ["End-to-End Retrieval with Learned Dense and Sparse Representations Using Lucene"](https://arxiv.org/abs/2311.18503) that came out of "playing around" with [ONNX](https://onnx.ai/).

Finally, a downside to working directly with me is that I might be "flaky" (from your perspective).
Working with URAs is relatively low on my list of priorities, and my attention is often on something more urgent, in which case I might not be able to respond quickly.

### Help Out One of My Graduate Students

My graduate students frequently work on exciting ideas and often look for help.
They'll post their ideas in the Slack channel, and you're welcome to contribute.
Typically, if you make significant contributions to a graduate student's project, you can expect to be included as a co-author on a paper.

URAs working with graduate students often happens without my knowledge:
It can either be a bug or a feature.
It's a bug because you don't get to interact with me.
It's a feature because if you "fail," I never know about it.

At a high level, working with my graduate students has the opposite tradeoffs as working directly with me:
You'll likely get to work on more "interesting" things, but they may be less well-formed and more vague.
And I'll only hear about your successes indirectly.

### Wow Us

Just that:
Do something cool.
Tell us about what you've done (in the Slack channel).
Impress us.
And instead of you working on "our" ideas, we'll help you with "your" ideas.

## Other Issues to Consider

### My Expectations

Students often ask what my expectations are: the answer is that I have _none_ (unless you sign up for a formal URA).
Yes, seriously.
If I don't hear from you after the first week, I won't think any less of you.
Life is short; everyone has conflicting demands on their time, and you should only work on something you're genuinely interested in.

My advice is simple: The more work you put in, the more you'll get out of working with me.
The single most important trait in successfully working with me, I believe, is being _self-motivated_; generally, a technical background isn't an issue &mdash; you got into Computer Science at Waterloo, after all.
With me, there are no deadlines.
How much interaction you get with me depends on how fast you _choose_ to work.
Starting with the "Initial Screen," once you finish something, I'll (try to) give you something else to work on.
If you finish it in a day, I'll give you something else to work on, and the same after that.
Those things will increase in complexity and provide you with more learning opportunities until you stop working (had enough) or decide you like them so much that you want a higher level of commitment (e.g., you want to do a Master's with me).

Note that, at least in the beginning, it may not be uncommon for me to assign the same task to multiple students.
Because of the voluntary nature of URAs, it is hard for me to gauge a student's level of interest or when (if ever) they will complete something.
Typically, I'll try to coordinate and reduce duplication on GitHub issues, but as tasks move on and off the critical path to my other projects, preemption is a possibility.

### Types of Tasks

You can get an overview of what each of my research projects is up to by looking at the recent commits, issues, etc.
Things to keep in mind when taking on tasks.
Our job is to find you tasks that:

+ Are concrete and can be clearly described;
+ Have clearly defined success criteria; and,
+ Provide learning opportunities for you.

You should answer whether you'd like to be on the critical path for our research or work on a side project.
The critical path refers to tasks that are blockers for our research, such as those needed for the next paper deadline.
A side project might be a new avenue of exploration that one is actively working on.
Both have their advantages and disadvantages.

Being on the critical path means a higher time commitment, and we'll be after you to deliver.
But this likely means you'll get more attention and engagement from me and my graduate students.
A side project is more likely open-ended, but you'll likely get less focused direction and mentoring.
For these, you'll have to take the initiative.

Another issue to think about is whether you wish to do a [_formal_ URA](https://cs.uwaterloo.ca/current-undergraduate-students/research-opportunities/undergraduate-research-assistantship-ura-program).
A formal URA gets paid, and I expect a commitment of ten hours per week.
I would consider it irresponsible if you disappear halfway through the semester.
So, if you're already taking six courses, a formal URA is probably not for you.
Most students work with me informally and come and go as they please.

A related question is: "What can I put on my resume?"
Don't put URA unless you do a formal URA.
However, I'm comfortable with more general descriptions such as "research assistant with Prof. Jimmy Lin."
But then you ask, "What's the threshold of my involvement to put it on my resume?"
That's your call.
I'm not going to police people's resumes (nor do I have time to).
However, remember that everything on your resume is subject to verification, so if an interviewer asks, "Oh, you worked with Prof. Lin; what did you work on?"
You'd better have a good answer.
(It's a small world, and there's a chance that the interviewer knows me personally or is a former student, etc.)
If the interviewer reaches out to me to check (this has happened before), I should have something meaningful to say.

## DL-fu and Muppet Trainer Character Development

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I present ugrads who want to work with me two-character class options: data wrangler or muppet trainer. Most want to be the latter these days.</p>&mdash; Jimmy Lin (@lintool) <a href="https://twitter.com/lintool/status/1387051481841373201?ref_src=twsrc%5Etfw">April 27, 2021</a></blockquote>

Many students want to become muppet trainers, and I have a relatively well-developed character development progression path.
Your mastery of deep learning skills progresses roughly along the following levels:

+ Level 1: You can run inference with an existing model.
+ Level 2: You can train an NN from scratch and get reasonable results (with existing code).
+ Level 3: You can make simple tweaks to an existing model.
+ Level 4: You can code up novel NN architectures.

The tasks I assign to URAs also tend to follow along this progression.
Levels 1 and 2 focus primarily on replication, although replicating training runs is often more complex than just replicating inference runs.
The beginning of level 3 typically involves asking you to help us with experiments by making simple parametric changes, e.g., hyperparameter sweeps.
It often follows relatively simple model tweaks (e.g., changing max to mean pooling) towards increasingly substantive model changes until reaching level 4.

## Transitioning from a URA to a Grad Student

In a way, being a good URA is easy: it's all about implementation chops.
You get assigned an issue; you resolve the issue.
Rinse. Repeat.

As a grad student, you must decide what issues are worth tackling.
In other words, answering a research question is often straightforward.
It's formulating a meaningful research question or deciding _which_ research question is interesting—that's the challenge.