# debugging

this will be the first post of a series, i don't how many will be at the end, so here we go:

first, shoutout to this talk https://www.youtube.com/watch?v=30jNsCVLpAE, this post and the talk will a have some common topics but with different purposes.

to this first post we'll consider a single system debug, we won't have any examples or codes, and we'll just *imagine* the situation, as in a real debug, 80% of the job is done without touching the computer.

what do you think when you think about debugging? finding an error? investigate a behavior? probably something around that.

but let's go one step back, when we're about to debug something we may only have a statement saying: something wrong is happening. probably from an angry customer, but let's stick do that *something wrong is happening*, we're dealing with computers then everything happens for a reason (or not, but we'll talk about it in another post).

if a button is doing something different than expected, where should we go? maybe we can go check the input?

that's a good start, we try that and then... we see the input is wrong, ok so where should we go now? go back to where this final function is called. but wait, we have two calls to that function in the system, things start getting messier and we're feeling we lost the FIO DA MEADA. what do we do now?

let's recap: we're clicking on a button which can do two things: A and B, but it's doing B when it should be A. and we only know the last function called and the result's wrong, let's say here the result is an error flag on a model, after looking more into it we find: the input is wrong, but we also found the function is called in two different pieces of the system.

now we have to figure out from where's it's being called, now instead of one bigger problem, we have two a bit smaller ones, how can we evolve that? let's take a look into the system X1 now, do we see any logs calling to this system on the same scenario (traceId or anything like that)? *or* can we reproduce the button click after adding some logs to it? okay we only have access to the logs, so we cannot reproduce the error (because this would make the post shorter), after checking the logs we see: from the system X1 everything's looking fine and the function is running, this moment you take a moment to look at the horizon and 72 deep breaths (you really need that, trust me), so let's go back to our logs, what do we see there after looking deeper: we see the function is called twice, we had only investigated the newest call, let's go to the oldest one.

taking a look now at the system X2, we see the input's wrong for our function from the start, ok now we can say we found the problem, but don't understand it, right? we're seeing the error behavior after the button click, but the function ran twice: first the wrong one, and then the success one. let's take a deeper look into the database row: we see the error column is filled, but so are the success fields too, both result cases (success and error) coexists on the database.

and now we have two choices, one is messier but quicker and the other one is classy but will take more time, pick your alignment charts:

### lawful good

we have to understand why is this happening, we're engineer and nothing passes by without we noticing, we'll keep the system as clean as possible but maybe we'll make the customer waits for a few more hours before the fix is released.

### chaotic good

we found the problem, let's change how we write both the success and the error, each one should erase the fields filled by the other. if it's success we erase the filled error fields, and the other way around. we'll make it engineering right later, but first we'll make it customer right first.


> I chose mine, what do I do now?
wait for the next post, this was only the first one.

# conclusion

thank you for reading the short story, for the sake of story none of our actions have any consequences in real world, that's why the chaotic good took that path.

and now we'll do a quick talk on the reasoning while debugging: we have to find a hole in a pipe, but the pipe is made of glass so we can see through it (check the logs), we see the problem: we're getting less water at the end than we're sending at the beginning (WE WON'T TALK ABOUT FLUIDS DYNAMICS HERE), we can measure how much water each step of the pipe has (we can check each function's input and output), we can do whatever search we like, we can pick random steps of the pipe to check the water (and god bless you in that process, but it can work), we can do a binary search: if at 50% of the pipe length we have the right amount, then the leak is occuring after that, this way we get rid of 50% of the candidates to be wrong, and then we can keep going on it.

a good debugger know what questions to ask a system, also know how to separece the system into blocks (like a pipe step above), but above all that he knows the problem is somewhere on the pipe, and someone'll find it be it in the next 5 minutes or in the next 5 hours, 5 days or, and god bless this souls, in the next 5 months. the problem exists somewhere, if our code is 100% right, the problem could be in the libraries we're using, if not there it can be on the browser, but it's somewhere. debugging is, before a code skill, an investigation one. and that's the most fun part of software engineering, creating one thing is amazing, but fixing a messy one? even better.