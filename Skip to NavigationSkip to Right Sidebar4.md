I map out every single file before coding and it changed everything



I've been building this ERP thing for my company and I was getting absolutely destroyed by complex features. You know that feeling when you start coding something and 3 hours later you're like "wait what was I even trying to build?"

Yeah, that was me every day.

The thing that changed everything
So I started using Claude Codeand at first I was just treating it like fancy autocomplete. Didn't work great. The AI would write code but it was all over the place, no structure, classic spaghetti.

Then I tried something different. Instead of just saying "build me a quote system," I made Claude help me plan the whole thing out first. In a CSV file.

Status,File,Priority,Lines,Complexity,Depends On,What It Does,Hooks Used,Imports,Exports,Progress Notes
TODO,types.ts,CRITICAL,200,Medium,Database,All TypeScript interfaces,None,Decimal+Supabase,Quote+QuoteItem+Status,
TODO,api.service.ts,CRITICAL,300,High,types.ts,Talks to database,None,supabase+types,QuoteService class,
TODO,useQuotes.ts,CRITICAL,400,High,api.service.ts,Main state hook,Zustand store,zustand+service,useQuotes hook,
TODO,useQuoteActions.ts,HIGH,150,Medium,useQuotes.ts,Quote actions,useQuotes,useQuotes,useQuoteActions,
TODO,QuoteLayout.tsx,HIGH,250,Medium,hooks,3-column layout,useQuotes+useNav,React+hooks,QuoteLayout,
DONE,QuoteForm.tsx,HIGH,400,High,layout+hooks,Form with validation,useForm+useQuotes,hookform+types,QuoteForm,Added auto-save and real-time validation
But here's the key part - I add a "Progress Notes" column where every 3 files, I make Claude update what actually got built. Like "Added auto-save and real-time validation" in max 10 words.

This way I can track what's actually working vs what I planned.

Why this actually works
When I give Claude this roadmap and say "build the next 3 TODO files and update your progress notes," it:

Builds way more focused code

Remembers what it just built

Updates the CSV so I can see real progress

Doesn't try to solve everything at once

Before: "hey build me a user interface for quotes" → chaotic mess After: "build QuoteLayout.tsx next, update CSV when done" → clean, trackable progress

My actual process now
Sit down with the database schema

Think through what I actually need

Make Claude help me build the CSV roadmap with ALL these columns

Say "build next 3 TODO items, test them, update Status to DONE and add progress notes"

Repeat until everything's DONE

The progress notes are clutch because I can see exactly what got built vs what I originally planned. Sometimes Claude adds features I didn't think of, sometimes it simplifies things.

Example of how the tracking works
Every few files I tell Claude: "Update the CSV - change Status to DONE for completed files and add 8-word progress notes describing what you actually built."

So I get updates like:

"Added auto-save and real-time validation"

"Integrated CACTO analysis with live charts"

"Built responsive 3-column layout with collapsing"

Keeps me from losing track of what's actually working.

Is this overkill?
Maybe? I used to think planning was for big corporate projects, not scrappy startup features. But honestly, spending 30 minutes on a detailed spreadsheet saves me like 6 hours of refactoring later.

Plus the progress tracking means I never lose track of what's been built vs what still needs work.

Questions I'm still figuring out
Do you track progress this granularly?

Anyone else making AI tools update their own roadmaps?

Am I overthinking this or does this level of planning actually make sense?

The whole thing feels weird because it's so... systematic? Like I went from "move fast and break things" to "track every piece" and I'm not sure how I feel about it yet.

But I never lose track of where I am in a big feature anymore. And the code quality is way more consistent.

Anyone tried similar progress tracking approaches? Or am I just reinventing project management and calling it innovative lol

r/ClaudeAI - I map out every single file before coding and it changed everything
Building with Next.js, TypeScript, Supabase if anyone cares. But think this planning thing would work with any tools.

Really curious what others think. This felt like such a shift in how I approach building stuff.