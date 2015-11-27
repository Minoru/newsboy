Hacker's Guide
==============

This document describes the architecture of Newsboy and is aimed at developers
willing to understand the code and contribute. You **don't** need to know any
of this to simply use the program.

Overview
--------

Newsboy consists of a number of components that communicate via message passing.

`API` processes users' requests and, upon consulting the other modules, sends
out responses. It should be noted that it doesn't directly implement any of the
public APIs supported by Newsboy. Instead, it provides a *meta-API*, if you
will, that other (possibly external and third-party) modules can translate into
anything they like.

