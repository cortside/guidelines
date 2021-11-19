 Things that should be added somewhere more formally/completely
 * don't use magic numbers.  endpoints should return strings for enums. also recommend persisting string for enum values in data store

Matt Wilkinson:spiral_calendar_pad:  8:29 AM
So I was treating the new endpoint like the get terms endpoint which is a POST.  Since it is calculating the values and not simply looking stuff up.  What is the defining characteristics in determining when to use GET vs POST?  I obviously don't know hahaha
Cort:spiral_calendar_pad:  8:31 AM
haha
8:31
good question
8:31
there is not a rule, so it's not "wrong"  --- or "right"
8:31
i would say that since the request is simple -- no body, i would use GET

Matt Wilkinson:spiral_calendar_pad:  8:31 AM
:exploding_head:

Cort:spiral_calendar_pad:  8:32 AM
you can not have a GET request with a body, so it would have to be something else in that case, like POST
