# Node.js的GC及性能分析

## Node.js的GC

## 性能分析

### V8内置分析器

V8分析器可以在程序执行期间定期运行记录程序栈的情况，并且包括jit编译这样重要的优化活动，需要注意的是该工具是在Node.js 4.4.0版本引入的。

最简单的使用，`node --prof xxx.js`

会在运行node的文件夹下生成*isolate-0xnnnnnnnnnnnn-v8.log*这样的文件。

文件内容实例：

```
v8-version,5,0,71,60,0
shared-library,"/usr/local/bin/node",0x100001000,0x1009f57dd
shared-library,"/System/Library/Frameworks/CoreFoundation.framework/Versions/A/CoreFoundation",0x7fffcb6ee830,0x7fffcb8dbdec
shared-library,"/usr/lib/libSystem.B.dylib",0x7fffdf27a989,0x7fffdf27ab5c
shared-library,"/usr/lib/libstdc++.6.dylib",0x7fffe0552e10,0x7fffe0593f23
shared-library,"/usr/lib/libDiagnosticMessagesClient.dylib",0x7fffdf03c013,0x7fffdf03ca26
shared-library,"/usr/lib/libicucore.A.dylib",0x7fffdf968ff4,0x7fffdfb225cb
shared-library,"/usr/lib/libobjc.A.dylib",0x7fffdff1d000,0x7fffdff3d570
shared-library,"/usr/lib/libz.1.dylib",0x7fffe0713b90,0x7fffe071ec45
shared-library,"/usr/lib/system/libcache.dylib",0x7fffe0733934,0x7fffe073654e
shared-library,"/usr/lib/system/libcommonCrypto.dylib",0x7fffe073889c,0x7fffe0741dea
shared-library,"/usr/lib/system/libcompiler_rt.dylib",0x7fffe0743ee4,0x7fffe0748ace
shared-library,"/usr/lib/system/libcopyfile.dylib",0x7fffe074c4fc,0x7fffe07525ee
shared-library,"/usr/lib/system/libcorecrypto.dylib",0x7fffe0754a80,0x7fffe07b7994
shared-library,"/usr/lib/system/libdispatch.dylib",0x7fffe07d8cf4,0x7fffe08003ee
shared-library,"/usr/lib/system/libdyld.dylib",0x7fffe080b314,0x7fffe080f25d
shared-library,"/usr/lib/system/libkeymgr.dylib",0x7fffe0810a2b,0x7fffe0810e80
shared-library,"/usr/lib/system/liblaunch.dylib",0x7fffe081ef65,0x7fffe081ef80
shared-library,"/usr/lib/system/libmacho.dylib",0x7fffe081fd3c,0x7fffe08249fb
shared-library,"/usr/lib/system/libquarantine.dylib",0x7fffe08262c3,0x7fffe0827b42
shared-library,"/usr/lib/system/libremovefile.dylib",0x7fffe082890c,0x7fffe0829c81
shared-library,"/usr/lib/system/libsystem_asl.dylib",0x7fffe082b8ac,0x7fffe08411ba
shared-library,"/usr/lib/system/libsystem_blocks.dylib",0x7fffe0843818,0x7fffe0843ceb
shared-library,"/usr/lib/system/libsystem_c.dylib",0x7fffe0845a20,0x7fffe08ca4ba
shared-library,"/usr/lib/system/libsystem_configuration.dylib",0x7fffe08d2edc,0x7fffe08d55f4
shared-library,"/usr/lib/system/libsystem_coreservices.dylib",
```


