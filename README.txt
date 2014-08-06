This module provides a Drush Command to delete expired cache_form entries.

This drush command is meant to be configured as a cron tab to take care of purging
cache_form table.

Usage:
drush ffct : Deletes expired cache_form entries. Limits to 1000 by default.
drush ffct 2500 : Deletes 2500 expired cache_form entries.

FAQ:

1) But there is already safe_cache_form_clear module. Why this one?
Because safe_cache_form_clear module does NOT build the query in the most 
optimal way. Due to limitations with using subqueries on Drupal DB API, 
the module builds a list of  cids  and adds an IN clause on the query appending the huge list.

Say if you are purging 40K entries, the resultant query will 
be 2.6 Million characters long!!

2) You mean  safe_cache_form_clear's code is bad?
NO. It is because of the limitation with using limit while using Drupal's DB API
that the author of the module chose to append Ids as such. 
This module overcomes it by running a delete query from db_query, 
which is *NOT* the most ideal way to do.


3) But, doesn't Drupal's cron delete the cache_form table automatically?
Ideally it does. And you would not require modules like this or 
safe_cache_form_clear since Drupal core's cron takes care of it.
BUT it works only AS LONG AS cache_flush_cache_form is set to a 
specific value (not 0). This cache_flush_cache_form being set to 0 is a known 
issue to happen when cache_form grows large. 

(https://www.drupal.org/node/1506196)  I would need further research 
on this, but I believe this value remains set to 0 after a failed cron run. 
So manually fixing this value once might help for a while but it the 
problem would probably reappear after a failed cron run sometime later.

4) Anything else that I should know?
The query on this module deviates from the standard practice of using the DB API
and does not use placeholder on the query to overcome certain limitations wrt using the DB API.
Make sure you are aware of it and evaluate any security risks before using it.


