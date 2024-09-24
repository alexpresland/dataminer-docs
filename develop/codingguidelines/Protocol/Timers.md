---
uid: ConnectorBestPracticesTimers
---

# Timers

## Number of timers

The number of timers in a protocol must be kept as low as possible, as every timer in a protocol creates an additional thread.

## Default timer speeds

Unless otherwise specified by the TAM, the following default timer speeds must be used:

1. Alarm and status parameters: 10 seconds.
1. Configuration parameters: 1 minute. (Set the execution frequency of the included groups when Data Display is opened to 30 seconds by setting the dataDisplay attribute to a value of 30000.)
1. Static data: 1 hour.
1. Each timer must have a default interval of 75 (ms).

## Timer interval restrictions

The interval of timers must be carefully chosen. For example, suppose a timer triggers every 10 seconds and initiates an operation that requires 20 seconds to complete. This can have a negative impact on performance (SLScripting, SLProtocol). The timer interval must be set to a value allowing every group included in the timer to complete before the timer triggers again. This can be verified with SLNet Client Pendingcalls and the element logging. Note also that a timer interval cannot exceed 24 hours, since a thread in SLWatchdog cannot be registered for longer than that.

The set timer time must not exceed 24 days.

## Starting/stopping timers

The use of conditions must always be favored over starting, stopping and restarting timers, but a condition on a group is favored over a condition on a timer, since the timer performs a regular check on that condition to see if it becomes true. In case an action is used to start or stop a timer, a timer that is already started must not be started again, as this could lead to issues.

## Timer content

The last group in a timer should be a group of type "poll", "poll trigger", or "poll action", to guarantee that the timer does not start again while not all groups of the timer have been executed yet.