# IntelliJ Idea

## Performance tweaking 
The VM configuration for the Idea is stored in the `~/.local/share/JetBrains/Toolbox/apps/IDEA-U/ch-1/<version.number>.vmoptions`.

Currently running on the following configuration:

```
-Xms2g
-Xmx2g
-XX:ReservedCodeCacheSize=1024m
-XX:+UseCompressedOops
-Dfile.encoding=UTF-8
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-ea
-XX:CICompilerCount=2
-Dsun.io.useCanonPrefixCache=false
-Djava.net.preferIPv4Stack=true
-Djdk.http.auth.tunneling.disabledSchemes=""
-XX:+HeapDumpOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
-Djdk.attach.allowAttachSelf
-Xverify:none
-XX:TieredStopAtLevel=1
-XX:CICompilerCount=2
-XX:Tier4MinInvocationThreshold=100000
-XX:Tier4InvocationThreshold=110000
-XX:Tier4CompileThreshold=120000
-XX:MaxInlineLevel=3
```