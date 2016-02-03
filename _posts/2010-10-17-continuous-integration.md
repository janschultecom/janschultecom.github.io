---
layout: post
title: Continuous integration
tags: [agile, automation, build, continuous integration, ci, quality, source control, test-driven development, tdd]
date: 2010-10-17
---
In this web article Martin Fowler, author of several popular books on topics such as refactoring and test-driven development, explains the agile practice of continuous integration (CI). CI is the process of committing and integrating frequently (at least once per day). This allows development teams to find bugs quickly and increase communication.

As Fowler points out, the basic idea is that team members divide their work into small chunks and commit them frequently, possibly every few hours. Developers do this by first checking out the current repository after they have finished their work chunk, then building and testing a local build and finally, if everything works fine, commiting the changes to the server. After commiting, the complete build is tested on the integration machine.

Obviously, CI requires a well-defined test suite, which is why CI works hand-in-hand with test-driven development methods. Additionally, very good automation mechanisms are necessary in order to avoid unnecessary maintenance overhead. Fowler devotes several sections to this topic.

Continuous integration has several advantages. Above all, it allows the development team to find bugs very quickly, as developers constantly test how their code interacts with the code from other developers.

As I have come across this topic recently in several presentations of different companies (among them Ericsson), who all name continuous integration as one of their major practices, I think this is a very interesting article with a huge relevance in the industry. It is definitly worth reading.

Link to the article: [Continuous integration](http://www.martinfowler.com/articles/continuousIntegration.html)
