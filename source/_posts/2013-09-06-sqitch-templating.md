---
layout: post
title: "Sqitch Templating"
date: 2013-09-06 13:44
comments: true
external-url: 
published: false
categories: [sqitch]
---

Yeseterday saw the v.980 release of [Sqitch], a database change management
system. The headline feature in this version is support for [MySQL] 5.6.4 or
higher. Why 5.6.4 rather than 5.1 or even 5.5? Mainly because 5.6.4 finally
added support for fractional seconds in `DATETIME` columns (details in the
[release notes]). This feature is essential for Sqitch, because changes often
execute within a second of each other, and the deploy time is included in the
log table's primary key.

With the requirement for fractional seconds satisfied by 5.6.4, there was
nothing to prevent usage of [`SIGNAL`], added in 5.5, to
[mimic check constraints] in a trigger. This brings the Sqitch MySQL
implementation into line with what was already possible in the Postgres,
SQLite, and Oracle support. Check out the [tutorial] and the accompanying
[Git repository] to get started managing your MySQL databases with Sqitch.

[Sqitch]: http://sqitch.org/ "Sane database change management"
[MySQL]: http://dev.mysql.com/ "The world's most popular open source database"
[release notes]: https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-4.html "Changes in MySQL 5.6.4 (2011-12-20, Milestone 7)"
[`SIGNAL`]: http://dev.mysql.com/doc/refman/5.5/en/condition-handling.html "MySQL 5.5 Reference Manual: Condition Handling"
[mimic check constraints]: http://stackoverflow.com/a/17424570/79202 "How do I get MySQL to throw a conditional runtime exception in SQL"
[tutorial]: https://metacpan.org/module/sqitchtutorial-mysql "A tutorial introduction to Sqitch change management on MySQL"
[Git repository]: https://github.com/theory/sqitch-mysql-intro "Sqitch MySQL Intro Sample Repository"

The MySQL support might be the headliner, but the change in v0.980 I'm most
excited about is improved template support. Sqitch executes templates to
create the default deploy, revert, and verify scripts, but up to now they have
not been easy to customize. With v0.980, you can create as many custom
templates as you like, and use them as appropriate.

<!-- more -->

A Custom Template
-----------------

Let's create a custom template for creating a table. The first step is to
create the template files. Custom templates can live in <code>\`sqitch
--etc-path\`/templates</code> or in `~/.sqitch/templates`. Let's use the
latter. Each template goes into a directory for the type of script, so we'll
create them:

``` sh Create template directories
mkdir -p ~/.sqitch/templates/deploy
mkdir -p ~/.sqitch/templates/revert 
mkdir -p ~/.sqitch/templates/verify
```

Copy the default templates for your preferred database engine; here I copy the
Postgres templates:

``` sh Copy default templates
tmpldir=`sqitch --etc-path`/templates
cp $tmpldir/deploy/pg.tmpl ~/.sqitch/templates/deploy/createtable.tmpl
cp $tmpldir/revert/pg.tmpl ~/.sqitch/templates/revert/createtable.tmpl
cp $tmpldir/verify/pg.tmpl ~/.sqitch/templates/verify/createtable.tmpl
chmod -R +w ~/.sqitch/templates
```

Here's what the default deploy template looks like:

``` sql Default deploy template
-- Deploy [% change %]
[% FOREACH item IN requires -%]
-- requires: [% item %]
[% END -%]
[% FOREACH item IN conflicts -%]
-- conflicts: [% item %]
[% END -%]

BEGIN;

-- XXX Add DDLs here.

COMMIT;
```

The code before the `BEGIN` names the template and lists dependencies, which
is reasonably useful, so we'll leave it as-is. We'll focus on replacing that
comment, `-- XXX Add DDLs here.`, with the template for a `CREATE TABLE`
statement. Start simple: just use the change name for the table name. In
`~/.sqitch/templates/deploy/createtable.tmpl`, replace the comment with these
lines:

``` sql Add CREATE TABLE statement to deploy template
CREATE TABLE [% change %] (
    -- Add columns here.
);
```

In the revert template, `~/.sqitch/templates/deploy/createtable.tmpl`, replace
the comment with a `DROP TABLE` statement:

``` sql Add DROP TABLE statement to revert template
DROP TABLE [% change %];
```

And finally, in the verify template,
`~/.sqitch/templates/verify/createtable.tmpl`, replace the comment with a
simple `SELECT` statement, which is just enough to verify the creation of a
table:

``` sql Add SELECT statement to verify template
SELECT * FROM [% change %];
```

Great, we've created a set of simple customized templates for adding a
`CREATE TABLE` change to a Sqitch project. To use them, just pass the
`--template` option to [`sqitch add`], like so:

``` sh Use the createtable template
> sqitch add widgets --template createtable -n 'Add widgets table.'
Created deploy/widgets.sql
Created revert/widgets.sql
Created verify/widgets.sql
Added "widgets" to sqitch.plan
```

Now have a look at `deploy/widgets.sql`:

``` sql Widgets Deploy Script
-- Deploy widgets

BEGIN;

CREATE TABLE widgets (
    -- Add columns here.
);

COMMIT;
```

Cool! The revert template should also have done its job. Here's
`revert/widgets.sql`:

``` sql Widgets Revert Script
-- Revert widgets

BEGIN;

DROP TABLE widgets;

COMMIT;
```

And the verify script, `verify/widgets.sql`:

``` sql Widgets Verify Script
-- Verify widgets

BEGIN;

SELECT * FROM widgets;

ROLLBACK;
```

[`sqitch add`]: http://metacpan.org/module/sqitch-add "Add a database change to the Sqitch plan"
