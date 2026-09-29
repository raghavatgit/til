# JVM G1 Garbage Collector Architecture

## Region-Based Heap
Instead of contiguous physical generations, G1 partitions the heap into 2,048 equal-sized regions (1MB - 32MB). Each region dynamically acts as Eden, Survivor, or Old generation.

## Mixed GC and Evacuation
G1 collects garbage based on pause time targets (`-XX:MaxGCPauseMillis`):
1. Concurrent Marking: Identifies regions with the highest proportion of dead objects (Garbage First).
2. Evacuation: Live objects from selected candidate regions are copied into clean survivor/old regions, defragmenting memory in-place during short Stop-The-World pauses.
