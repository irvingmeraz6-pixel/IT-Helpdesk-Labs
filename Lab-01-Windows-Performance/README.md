# Lab 01 — Windows Performance Troubleshooting

**Lab Type:** Simulated Tier 1 Help Desk Support  
**Operating System:** Windows 11  
**Tools:** Windows Task Manager  
**Status:** Completed

## Objective
Investigate a simulated slow-PC complaint by examining system resource usage and startup applications.

## Troubleshooting Steps
1. Opened Task Manager and inspected CPU, memory, disk, and network usage.
2. Sorted running processes by memory consumption.
3. Reviewed installed RAM and available memory.
4. Examined startup applications and their impact ratings.
5. Identified Steam as an enabled, high-impact startup application.
6. Disabled Steam's automatic startup.
7. Restarted Windows and verified the change persisted.

## Findings
- CPU usage: approximately 3–4%.
- Memory usage: approximately 31–32%.
- Installed memory: 32 GB DDR4.
- No obvious CPU or memory bottleneck was observed during testing.
- Steam was enabled with a High startup impact rating.

## Resolution and Verification
Disabled Steam's automatic startup without uninstalling the application. After restarting Windows, verified that Steam remained disabled and did not launch automatically.

## Lessons Learned
- Resource usage should be measured before making performance recommendations.
- High startup impact does not necessarily mean an application is causing a performance problem.
- Disabling unnecessary startup applications is a reversible configuration change.
- A technician should verify changes and document the results.

## Outcome
Successfully changed and verified a Windows startup setting. Overall startup speed improvement was not measured.