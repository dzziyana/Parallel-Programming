# Hardware support for atomic operations
Both **TAS** and **CAS** are **Read-Modify-Write** operations

> [!Info]
> CAS can be implemented wait-free on hardware!
> #Slides-Lecture-22
> 

![[TAS-CAS.png]]
Java does not have a true TAS operation :P
![[no true TAS in Java.png]]

# TAS Lock and CAS Lock - [[Spinlock]]
![[TAS lock and CAS lock.png]]
## Problem mit TAS Lock
lock-holder: `state = true` -> laufend neue Write-Action
lock-contenders: Kopie von `state` laufend invalidiert, jedes Mal eine neue Kopie von Main Memory...
- sowieso ein langsamer Ansatz
- extra Wartezeit weil (wenn?) Bus-Capacity überschritten
=> TAS-Lock funktioniert, aber schreckliche Performance

![[Memory illlustration.png]]

## Lösung - [[TATAS Lock]] 

## Load Linked / Store Conditional

