---
title: Belux vACC Plugin Survey
description: Belux vACC just filled in a survey about their plugins. Let's explain
tags: vatsim euroscope cpp
---

Belux vACC has seen quite some changes in its plugins this last year and I wondered what the vATCOs thought of this.
I made a very short survey, published it to our ATCOs and analysed the results.

First and foremost, let's set some ground truths about responses.
We pinned the link and pinged everyone in discord, but not all our members are always this active in discord.
Our roster counts around 30 vACTCOs and I got 16 responses on the survey.
Half of the target population is not a bad reach for a survey, but 16 samples still make for difficult analysis.

## General information

The general section was used to ask about some preferences and very broad feature suggestions.
The feature suggestions will be discussed at the end of this article.

One of the preferences surveyed, was how much automation people actually want and it turns out Belux broadly agrees that striking a good balance is important here.
Half of respondents marked the halfway point between more and less automation, the other 8 neatly divided along both sides of the spectrum.

Similar agreement can be found when we asked about the value of realism.
We asked to rate how much they valued realism on a scale of one to five, five meaning most deeply valued.
Overwhelmingly, the responses fall onto the five here, with only one ATCO giving a three and none choosing two or below.

This gives some great guidance about the future for our software suite.
We should be sticking to what we already do so well and keep things close to real life Skeyes systems,
with a good balance between brains and automation.

## vSMR

vSMR is a plugin originally written by pierr3 for French vACC. It has since through Nicola passed into my hands.
It is a very, very close approximation of the ground radars used in real towers throughout our airspace.
This focus in realism is reflected in what the controllers at Belux say should be the focus for vSMR;
over 80% state we should strive to be as close to real as possible. 
12.5% stated they'd prefer is vSMR remained largely as is, with future work focusing on stability.
It has to be noted here that we should not lose sight of things that are unique to VATSIM and how we can accomodate those,
even if it does not exactly match real life.

When asked to clarify their specific likes,
most point towards the destination gate indicator, the realism and the clear colour-coded tags.

The documentation and usage of vSMR was found to be generally quite clear.
Most confusion seems to be around the usage of its DCL facility.

Reading between the lines, one starts to notice that to controller the line between vSMR and the Belux plugin can become quite blurred,
this is especially apparent when asked about dislikes.
The main complaint about vSMR appears to be that during busy times the screen can get quite cluttered with tags, 
even if pro mode is enabled. We'll get back to this in the section on feature requests.

### Taxi Route Planning

The real-life ground radar features several facilities for planning and displaying taxi routes and hold short instruction for aircraft.
The idea to replicate this has been floating around for a while,
so a question was inserted in this survey to ask how controllers feel about it.

Here we see that a quarter of respondents indicated a clear dislike for the idea,
with in total half of surveyed controllers indicating they would not use such a feature.
The remaining answers generally indicate liking it, if properly implemented, quick to use and uncluttered.

These responses show that implementing this may not be the best use of our time.
However, we may still go ahead with implementing a subset, such as a visual clearance limit on the ground.

## Belux Plugin

The Belux plugin does a lot, from gate management to SID and initial climb assignment, it's a true workhorse for us.
Generally these features are well liked, though seemingly not always well understood.
The median self-reported understanding of this feature is four out of five,
but confusion becomes apparent in looking at the dislikes.

There, we find complaints about changing assigned gates being hard or commands being long.
These commands often have shorthand forms and where they don't, users can use alias commands to make this easier.
In the specific case of assigning gates, we have a tag item specifically for this purpose.
It is by default shown in the arrival list, but users may add it to the departure list as well.
The gate can also be changed in flight strip field 5, though this does not update the website.

For the person who reported not receiving SID suggestions in ELLX,
please contact me as this is not intended behaviour and likely a bug on your side.
You also mentioned you'd like to manually override SIDs "in the header",
but this suggestion is not clear to me at all.

## Suggestions

I received a great deal of suggestions for the future and features of these plugins.
Let's go through them.

We could provide a system for silent coordination of runway crossings between ground and tower.
Suggestions for how this could be made most practical are always welcome.

A slot assigner for events was requested, however this request is not entirely clear to me.
The VATCAN plugin already exists to check pre-registered slots.
If you are asking about assinging CTOTs when times are busy and the going gets tough,
then I still don't know what to tell you.
This is a complicated feature, that would likely best be a separate plugin and someone with a better understanding of delays and flows to help implement.

Several solutions were proposed for the ground tag clutter issue.
One is only showing tags for aircraft on your frequency.
Sadly, this is not possible. Tracking what frequency a pilot is listening to can only be done manually through the `.inf` command.
Perhaps an (optional) filter based on assumed / free state could be implemented if desired.

Making the aircraft symbols on the ground move smoother is similarly impossible,
this time because Euroscope does not support VATSIM Velocity.

Several requests were made about things to add to the ground TAGs.
Adding a click action to open the flight plan window should be possible,
if I can find a spot that does not yet have a different click action.
Changing the gate from the tag is unfortunately not an option due to technical limitations,
as is changing the STS field from the tag. There also is simply no space left for the STS field.
If I can wrap my head around the graphics for it, making the tags non-rectangular as IRL is possible.

Showing a hint with the aircraft callsign on hover when a full tag is hidden due to pro mode is possible
and can be implemented if it's broadly desired.
Let me know your wishes in the discord.

An RDF (Radio Direction Finder) for the ground was requested, as already exists for the approach and area control screens
to show a rough circle around the origin of a transmission.
Sadly real RDF systems are quite imprecise, as you can see by that green circle being sometimes significantly larger than the entirety of Brussels Airport.
Therefore, such a feature is not all that realistic and won't be implemented.
You can always use the AFV window to see last callsign if you wish to "cheat".

One controller requested a feature that would automatically start timers depending on aircraft state;
for example when an aircraft has the STS set to DEPA and accelerates past a specific speed.
This could be useful and _may_ be implemented, probably in a separate plugin.

Hierarchically sorted lists have been discussed before, but it's worth explaining this again in the _wishful thinking_ section.
Euroscope lists treat every field as a string of text and sort it alphabetically, resulting in weirdness.
You also cannot sort on several fields hierarchically.
Plugins could in theory draw their own lists from scratch with any sorting they can think of,
but they are unable to fetch tag items provided by other plugins and thus cannot show this in these custom lists.
This makes the entire feature a bit useless and thus I must disappoint.

Continuing with pipedreams: a proper expected traffic window.
In a world where we have access to all information about a flight, this is already difficult.
You can see this difficulty when using something like AMAN,
which will show dramatic jumps in estimated arrival time even if an aircraft is already well in your visibility range.
Outside your visibility, this becomes even harder, as we somehow have to compute ETA based on what the pilot said there off-block time and cruise speed would be,
combined with an educated guess for climb speed and hoping no delay vectors occur which you are unaware of.
We could somewhat augment these guesses by using the VATSIM datafile,
but even then we are largely limited to dividing the distance between their first known fix and current position by current reported speed.
In a world where EOBT times and cruise speeds are +90% accurate,
routing for the entire world is known and UNICOM_CTR behaves sensibly,
we could get quite close to a real expected traffic prediction.
Until then, we'll keep having rather dodgy best guesses.

Finally, the request for better pilots has been forwarded to Santa.
