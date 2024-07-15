---
aliases: []
title: Frontend Troubleshooting
date created: Monday, May 13th 2024, 12:43:43 pm
date modified: Monday, May 13th 2024, 12:44:33 pm
tags:
  - PCORnet_Projects_DSpace_Logs
  - PCORnet_Projects_DSpace
status: 
project: 
tech: 
---

## User Interface Never Appears (no Content appears) or "Proxy Server Received an Invalid response"

I am trying now to call `yarn start:dev` as per the [troubleshooting FAQ](https://wiki.lyrasis.org/display/DSDOC7x/Installing+DSpace#InstallingDSpace-CommonInstallationIssues:~:text=REST%20API%20(backend)-,User%20Interface%20never%20appears%20(no%20content%20appears)%20or%20%22Proxy%20server%20received%20an%20invalid%20response%22,-Chances%20are%20your).

Update: that was pointless

```
<--- Last few GCs --->

[475192:0x55bfe6f82390]   190859 ms: Mark-sweep 2014.9 (2087.9) -> 2002.9 (2092.4) MB, 1556.6 / 0.0 ms  (average mu = 0.433, current mu = 0.147) allocation failure; scavenge might not succeed
[475192:0x55bfe6f82390]   193035 ms: Mark-sweep 2018.9 (2092.4) -> 2006.8 (2096.2) MB, 2141.1 / 0.0 ms  (average mu = 0.222, current mu = 0.016) allocation failure; scavenge might not succeed

<--- JS stacktrace --->

FATAL ERROR: Reached heap limit Allocation failed - JavaScript heap out of memory
 1: 0x55bfe429c3f4 node::Abort() [ng serve --host localhost --port 4000 --serve-path / --ssl true --configuration development]
 2: 0x55bfe412d500 node::OOMErrorHandler(char const*, bool) [ng serve --host localhost --port 4000 --serve-path / --ssl true --configuration development]
 3: 0x55bfe44a4ee4 v8::Utils::ReportOOMFailure(v8::internal::Isolate*, char const*, bool) [ng serve --host localhost --port 4000 --serve-path / --ssl true --configuration development]
 4: 0x55bfe44a51a6 v8::internal::V8::FatalProcessOutOfMemory(v8::internal::Isolate*, char const*, bool) [ng serve --host localhost --port 4000 --serve-path / --ssl true --configuration development]
 5: 0x55bfe4669c99  [ng serve --host localhost --port 4000 --serve-path / --ssl true --configuration development]
 6: 0x55bfe4682731 v8::internal::Heap::CollectGarbage(v8::internal::AllocationSpace, v8::internal::GarbageCollectionReason, v8::GCCallbackFlags) [ng serve --host localhost --port 4000 --serve-path / --ssl true --configuration development]
 7: 0x55bfe465ca49 v8::internal::HeapAllocator::AllocateRawWithLightRetrySlowPath(int, v8::internal::AllocationType, v8::internal::AllocationOrigin, v8::internal::AllocationAlignment) [ng serve --host localhost --port 4000 --serve-path / --ssl true --configuration development]
 8: 0x55bfe465dc58 v8::internal::HeapAllocator::AllocateRawWithRetryOrFailSlowPath(int, v8::internal::AllocationType, v8::internal::AllocationOrigin, v8::internal::AllocationAlignment) [ng serve --host localhost --port 4000 --serve-path / --ssl true --configuration development]
 9: 0x55bfe4642193 v8::internal::Factory::NewFillerObject(int, v8::internal::AllocationAlignment, v8::internal::AllocationType, v8::internal::AllocationOrigin) [ng serve --host localhost --port 4000 --serve-path / --ssl true --configuration development]
10: 0x55bfe49d4e47 v8::internal::Runtime_AllocateInYoungGeneration(int, unsigned long*, v8::internal::Isolate*) [ng serve --host localhost --port 4000 --serve-path / --ssl true --configuration development]
11: 0x55bfe4df0bb9  [ng serve --host localhost --port 4000 --serve-path / --ssl true --configuration development]
[nodemon] clean exit - waiting for changes before restart
```

This did not have anything to do with the issue I have when I call `./dist/server/main.js`. So I don't know why it's giving an OOM error but that's only when I try to test it.