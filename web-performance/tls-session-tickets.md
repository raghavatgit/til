# Stateless TLS Session Tickets (RFC 5077)

## Problem Statement
Server-side TLS session caching (`Session ID`) requires distributed session caches across load-balanced server fleets.

## Solution
The server encrypts the master secret into a `Session Ticket` using a secret Session Ticket Encryption Key (STEK) and sends it to the client. On reconnection, the client presents the ticket; the server decrypts it locally without accessing a distributed database.
