# Spring Data Tips and Practices.

Greetings and welcome to the Spring Data Tips and Practices Zone! Where we hope to provide solutions to our common problems relating to Spring Data and Hibernate.

The current repository is updated each week to finish the things.
Let me know if you have any questions or suggestions.

## Table of Contents
* [Entity and Association](#entity-and-accosiation)
* [Fetching and Queries](#fetching-and-queries)
* [Batch Operations](#batch-operations)
* [Configuration and Connection Pool](#configuration-and-connection-pool)
* [Monitoring and Audeting](#monitoring-and-audeting)

### Entity and Accosiation

1. what's the best way to implement a bidirectional @OneToMany association?
   Imagine we want to map a User entity to its accounts.
the following rules are noticeable to have a bidirectional @OneToMany association:
  **We must always cascade from parent-side to child-site**
  
  
