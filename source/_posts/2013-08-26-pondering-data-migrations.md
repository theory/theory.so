---
layout: post
title: "Pondering Data Migrations"
date: 2013-08-26 11:38
comments: true
external-url: 
categories: [sqitch, data]
---

I've been thinking about data migrations. I love how well [Sqitch] works for
*schema* changes, but so far I've avoided the issue of *data* changes. Some
data ought to be managed by the deployment process, rather than by end-user
applications. Lists of countries, for example. Yet none of our Sqitch-managed
databases at [work] include `INSERT`s, `UPDATE`s, or `DELETE`s in deploy
scripts. Why not? Two reasons:

1. These are mainly Postgres ports of existing Oracle databases. As such, I've
   written independent migration scripts that use [`oracle_fdw`] to copy data
   from Oracle. It made no sense to commit hard-coded changes to the deploy
   script, even for static data, as it was all to be copied from the old
   production Oracle databases -- often months after I wrote the migrations.

2. These projects include extensive [pgTAP] unit tests. These tests expect to
   run many times against an empty database with no side effects. Having the
   different data in testing than in production increases the likelihood of
   harmful behavioral variations. Better to expect *no* data in unit tests, to
   free to focus on units of behavior without regard to preexisting data.

Neither of these is a hard and fast rule. The Oracle migration code could have
gone into deploy scripts, but then the data might vary between unit test runs,
making test maintenance a pain. Worse, it would mean that `oracle_fdw` would
forever after have be part of the deployment process, even after our Oracle
database was long gone, as Sqitch deployments cannot be removed once they have
gone to production. Add in the fact that I never got `oracle_fdw` to run on my
Mac, and overall it was just too much hassle. I never seriously considered it.

I also could have included non-Oracle static data in the deploy scripts -- and
maybe for green field database development that makes sense. Yet it's hard for
me to give up the idea of an empty database for unit tests. Philosophically, I
would rather keep data definition separate from schema definition, paralleling
the separation of data and presentation, as commonly exercised in application
development.

Deploy Hooks
------------

I think the best way to solve for the one-time migration requirement is
[deployment hooks]. The idea is similar to [Git hooks]: Before or after any
`sqitch deploy` (or `revert`), one or more hook scripts can be run. The
original idea was to allow a script to be run after every `deploy` to ensure
some higher level of consistency. For example, a post-deploy hook might grant
privileges on all tables in a database to a particular user. Another might run
`VACCUM; ANALZYE;` after every deploy.

But we could also use it for one-time data migrations. An option to `sqitch
deploy` will disable them, to avoid running them when building a test
database, for example. Once the migration has been run in production, we just
delete the hook script from the project. Sqitch won't record hook executions,
so adding or removing them would be no problem.

I like this approach, as it enables the running of a migration script to be
automated by Sqitch itself, but does not change the behavior or functionality
of Sqitch itself. And it's more generally useful. Hell, I might want a deploy
hook script that sends an email notification announcing a deployment. There
are all kinds of things for which hooks will prove useful.

Static Data Maintenance
-----------------------

For our current databases, in addition to one-time data migrations, we have
been maintaining changes to static data via independent SQL scripts that must
be run manually after a deployment. By "static," I mean data that must be
managed as part of the deployment process, rather than by applications that
use the database. A list of country codes, for example. Or a list of
attributes a user can set on an object.

It might make sense to store such changes in Sqitch deploy scripts, especially
if there are no other dependencies. A table mapping country codes to their
names has no foreign key constraints to other tables. One can `INSERT`,
`UPDATE`, or `DELETE` rows on the table in a deploy script and have it just
work. The overhead for updating unit tests for such changes would also likely
be relatively low, since changes are rare and the dependencies nonexistent.

Other data is trickier. Our core configuration database defines attributes
users can tweak. Two wrinkles complicate the maintenance of these attributes:

1. The `atributes` table has a foreign key constraint to the `users` table, to
   identify the users who add attributes to the database. Yet in a new
   database, the `users` table is empty -- Sqitch creates no users. The app
   defines users but not attributes. Chicken, meet egg.

2. In production, an Oracle migration populated the `attributes` table.
   Consequently, none of those attributes were defined in Sqitch deploy
   scripts. We can add attributes via Sqitch in the future, but then we don't
   have the canonical list of all attributes defined in our Sqitch project.

I could still use Sqitch migrations for params data, of course. The deploy
script could check to see if there are users, and if not, create one before
adding params. Then the tests would need to be adjusted to expect that one
user to be present. Arguably, no such foreign key constraint should exist in
the `attributes` table, since they are always managed by the deploy process,
rather than someone logged into the app. Perhaps it should just be dropped.

Either way, this hypothetical deploy script could insert only new attributes,
or check existing attributes and insert only those not already present.
Naturally tests would need to also be updated to account for these changes.

Or I could use the proposed deploy hooks to run these conditional updates.
Seems silly to have to let them happen every time any changes are deployed to
the database, though.

It all seems a tad inconvenient to do all this conditional deployment just to
use the existing Sqitch deploy convention. I would still rather keep data
separate from presentation.

What's the alternative?

### A Compromise ###

I can think of no alternative that does not complicate Sqitch. Ultimately,
these data migrations are exactly like schema changes: Sqitch must execute
them in the proper order relative to other changes, record successful or
failed deployment, and be able to revert them when required. The only
difference is what's defined in them: data modification, rather than
definition.

I propose a compromise, then: we should go ahead and manage all data that
should be part of the deployment process in Sqitch changes, but such changes
should contain *only* the data. Schema definition should go into other
changes. For our list of countries, for example, we might have a change named
"countries", which creates the `countries` table, and another, named
"country_data", which inserts the appropriate data into that table. Where
necessary and appropriate, these changes can use conditional paths to bring
the data up-to-date relative to what's currently deployed, such as when we
migrated data from Oracle.

Or when one must deal with side-effects, such as foreign key constraints.
Honestly, I think it appropriate to try to remove such side effects from
deployment-managed data. For tracking the user or users who added data to a
database, one can use the tools of the source code repository (`git log`, `git
blame`) to determine who's responsible. Other types of side-effects may be
more necessary, but to the extent possible, deployed data should be as
free-standing as possible.

Which means unit tests simply must expect the data to be there, and be updated
when that data changes. We are, after all, talking about infrequently-updated
data, I hope. Frequently-updated data should have separate interfaces provided
by applications to change the data.

[Sqitch]: http://sqitch.org/
[work]: http://iovation.com/
[`oracle_fdw`]: http://pgxn.org/extension/oracle_fdw
[pgTAP]: http://pgtap.org/
[instant client]: xxx
[deploy hooks]: https://github.com/theory/sqitch/issues/96
[Git hooks]: xxx (githooks)
