# Hardware support for atomic operations

![[Pasted image 20240613130444.png]]
Java hat keine TAS-Operation :P
![[Pasted image 20240613131004.png]]

# TAS Lock and CAS Lock
![[Pasted image 20240613130950.png]]
	#TODO *? why does the acquire method in the TAS lock wait while the lock is 0?*

## Problem mit TAS Lock
lock-holder: state = true -> laufend neue Write-Action
lock-contenders: Kopie von state laufend invalidiert, jedes Mal eine neue Kopie von Main Memory...
- sowieso ein langsamer Ansatz
- extra Wartezeit weil Bus-Capacity überschritten
=> TAS-Lock funktioniert, aber schreckliche Performance

![[Pasted image 20240613131911.png]]

## Lösung - [[TATAS Lock]] 

## Load Linked / Store Conditional

