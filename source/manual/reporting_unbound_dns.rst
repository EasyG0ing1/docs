======================
Reporting: Unbound DNS
======================

Starting from OPNsense 23.1, users are able to gain insight into DNS traffic passing through their Unbound DNS resolver
using the reporting tool under :menuselection:`Reporting --> Unbound DNS`.

All data presented here is kept on the system for a total of 7 days, creating a rolling window into DNS traffic without
allowing the system to take up boundless storage space.

-------------------------
Overview
-------------------------

The overview tab shows high-level DNS traffic data.

**Counters**

* The total amount of queries Unbound has handled, starting from the moment as reported above the counters.
  This will either be from the moment the gathering of statistics has been enabled, or up until the last 7 days.
  Keep in mind that the counter is as seen from the incoming side, and will increase regardless of the type
  of response returned.
* The amount of queries Unbound has successfully resolved. This counter does not distinguish between forwards or
  recursion, and excludes every other response type, such as responses from cache, local-data or a local policy
  such as a blocklist.
* The amount of queries Unbound has blocked. This is either because a queried domain was part of a blocklist,
  or part of a user-configured exact match as configured in :menuselection:`Services --> Unbound DNS --> Blocklist`.
* The size of the current blocklist (if any). This will equal the total amount of domains listed inside all the
  active blocklists.

Every query counter shows the percentage as part of to the total amount of queries.

.. Note::

    Adding up both the blocked and resolved queries does not equal the total amount, since the amount of
    responses from cache, local-data and other possible sources such as Unbound itself on e.g. a SERVFAIL are not
    shown.


**Graphs**

Also included in the report are two DNS traffic graphs, the first one being the query graph, and the second one
being the client graph. Both graphs show the amount of **incoming** queries over a selectable span of time.
The query graph also shows the amount of blocked queries. You can hover over the dots in the client graph
to see which client it is, as well as the amount of queries associated with this client.

Both the query and client graph have the option to display the data on a logarithmic scale in order to catch outliers
properly while preserving your perspective of the normal flow of traffic.

**Top domains**

On the bottom of the page the top 10 of both passed and blocked queries are shown. This includes the amount a domain
has been requested, as well as a percentage of passed or blocked requests respectively. If you have blocklists enabled,
you are also able to explicitly block or whitelist a specific domain from this top list with the click of a button.
The relevant domains will show up in :menuselection:`Services --> Unbound DNS --> Blocklist`, under "Whitelist Domains"
or "Blocklist Domains".

-------------------------
Details
-------------------------

The details tab shows a livefeed of **completed** queries along with reply information.
You can refresh the list by clicking the refresh button on the top right of the screen. In it you can find:

* Which client queried which domain with its associated DNS record type.
* The action taken by Unbound, this can either be pass, block or drop. The latter only occurs when a query could
  not be serviced due to an internal error.
* The source of the response. This can be either Recursion, Local, Local-data or cache. Local refers to a decision
  made by Unbound to either block or drop the query. Local-data refers to the custom host overrides and its associated
  aliases.
* The return code of the DNS query. Refer to the
  `IANA DNS Parameters <https://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml#dns-parameters-6>`__
  for its meaning.
* If recursion is involved, how long in milliseconds it took to resolve a domain.
* The TTL of the final answer. Answers from recursion will always contain an upstream-defined TTL value, while
  answers from cache will show a snapshot of the remaining cache TTL value before recursion would have to take place again.
  Please note that TTL behaviour can be largely dependent on the settings used in :menuselection:`Services --> Unbound DNS --> Advanced`.
* The blocklist used if a query was blocked.
* Either a block or whitelist action button, which can be used in the same way as described above for the "Top domains" in the
  overview section. Please note that this column will not appear if blocklists are disabled.