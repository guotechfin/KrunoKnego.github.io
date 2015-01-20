---
layout: post
title: "Dealing with MySQL foreign key checks"
category: 
date: " 2015-01-07 16:25:40"
tags: []
---

## Why disable foreign key checks!?

One cannot speak of relational databases without mentioning foreign keys as they are integral part of those kind of databases.
But sometimes when you're using MySQL you'll find yourself in need of temporarily disabling foreign key checks.
For example imagine a situation where you're continuously trying to import a MySQL table but on each try you get FK error.
Now that might be frustrating and you know it should work but only if you could somehow disable those pesky foreign keys if only
for a moment.

## How to disable FK?

Disabling foreign keys is very simple. If you wish to disable FK checks for only one session then you'd use the
following command <code>SET FOREIGN_KEY_CHECKS = 0</code>.
When you finished executing query that was giving you errors set back the FK checks to true <code>SET FOREIGN_KEY_CHECKS = 1</code>.

If you wish to disable FK checks not just for the current session but globally then you'd have to add GLOBAL keyword like this:
<code>SET GLOBAL FOREIGN_KEY_CHECKS = 0</code>. Just remember same as before once you've finished with execution of
your queries turn it back on <code>SET GLOBAL FOREIGN_KEY_CHECKS = 1</code>.

## Read more

* [mysql-set-foreign-key-checks](http://stackoverflow.com/questions/8538636/mysql-set-foreign-key-checks)
* [mysql-disable-foreign-key-checks-or-constraints](https://gauravsohoni.wordpress.com/2009/03/09/mysql-disable-foreign-key-checks-or-constraints/)
* [how-to-temporarily-disable-a-foreign-key-constraint-in-mysql](http://stackoverflow.com/questions/15501673/how-to-temporarily-disable-a-foreign-key-constraint-in-mysql)

