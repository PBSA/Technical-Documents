# Introduction to BOS

The Bookie Oracle System, or BOS, is a unique decentralized sports feed oracle system originally designed for the BookiePro dApp.

Unlike traditional centralized sports betting application that rely on perhaps one or two data feeds, Bookie through BOS has the potential for almost unlimited data feeds through the use of data proxies.

At its simplest, a data proxy is just middleware that consumes it's own data feed \(usually from a commercial supplier\) then parses and normalizes that data before sending it to BOS.

Since BOS supports multiple data proxies, it therefore supports multiple data feed providers.

This is where BOS becomes truly decentralized. There is no single source of truth as far as the sports data is concerned. BOS, either automatically or through manual intervention by Witnesses requires a consensus of at least 2 approvals in most cases, before processing incident/event data.

For example:

A soccer game takes place between two teams and there are three data proxies interfaced to BOS.

Each data proxy send the result of the game to BOS, two send the result as 2 - 1, but one sends the result as 2 - 2











## High Level Structure

![](../.gitbook/assets/bos-flow.jpg)

## 

