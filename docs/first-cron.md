---
id: first-cron
title: Your first Cron
---

Now that we're storing requests, we can use a scheduled job (a "Cron", named after the Unix cron utility) to create a report of all requests per day.

1. Hit the plus (+) button (or use the omnibox) to create a new Cron. Set it to run daily, and with the name DailyReport.

![assets/gettingstarted/crondaily.png](assets/gettingstarted/crondaily.png)

2. To know if a request was executed today, we compare it to the current time. Let's
   assign the current time in seconds to a variable `timeSeconds`. The current
   time in seconds is `Date::now`, piped into `Date::toSeconds`. (Note: to
   create a pipe, type `|>` after the `Date::now` function).

   Run the `Date::now` function to see a live value for your code.

![assets/gettingstarted/cron_timeseconds.png](assets/gettingstarted/cron_timeseconds.png)

3. Next, we'll need to filter the reports to just today's reports. Set `allRequests` to `DB::getAll Requests`. Run the function to see the live value for `allRequests`.

![assets/gettingstarted/allrequests.png](assets/gettingstarted/allrequests.png)

4. To see only today's requests, use `List::filter`. `List::filter` takes two arguments - the list to filter (in this case `allRequests`) and an anonymous function that determines if each element of the list element should be includes in the new list. `List::filter` is similar to `List::map` and the concept is explained in more detail [here](https://darklang.github.io/docs/functional-aspects).

![assets/gettingstarted/listefilter.png](assets/gettingstarted/listfilter.png)

5. We want to keep only reports that occurred today, meaning we want to compare the report's time to `timeSeconds` that we created earlier. Let's convert the time of the request into seconds so that both times have the same units.

![assets/gettingstarted/requestseconds.png](assets/gettingstarted/requestseconds.png)

6. Next, we calculate the difference from the current time; this tells us if it happened today (in the last 86,400 seconds).

![assets/gettingstarted/comparison.png](assets/gettingstarted/comparison.png)

7. To see this work, it's helpful to have a recent request. If you paused in the tutorial, re-run the `HTTPClient::post` function from the REPL. Then, when you place your cursor in `todayRequests` or `List::filter` you'll see a list of requests from the last 24 hours.

![assets/gettingstarted/todayrequest.png](assets/gettingstarted/todayrequest.png)

8. Finally, let's emit today's requests to a background worker, using the `emit` keyword. `emit` takes two arguments, and in this case we're sending `todayRequests` to a not-yet-created worker named `storeReport`.

![assets/gettingstarted/emit.png](assets/gettingstarted/emit.png)

9. To have our first report run, hit the "replay" button in the upper right of the Cron. This creates a 404 in the sidebar for the Worker `storeReport`.
