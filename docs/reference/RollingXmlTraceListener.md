# RollingXmlTraceListener Class

Trace listener that writes E2ETraceEvent XML fragments to a text file, rolling to a new file based on a filename template (usually including the date).

## Installing

Install via NuGet:

* PM> **Install-Package [Essential.Diagnostics.RollingXmlTraceListener](http://www.nuget.org/packages/Essential.Diagnostics.RollingXmlTraceListener)**

## Remarks

A rolling log file is achieved by including the date in the filename, so that when the date changes a different file is used.

Available tokens are DateTime (a UTC DateTimeOffset) and LocalDateTime (a local DateTimeOffset), as well as ApplicationName, ProcessId, ProcessName and MachineName. These use standard .NET format strings, e.g. "Trace{DateTime:yyyyMMddTHH}.svclog" would generate a different log name each hour.

The E2ETraceEvent XML fragment format can be read by the Service Trace Viewer tool.
	
## Config Attributes

| Attribute | Description |
| --------- | ----------- |
| initializeData | Template file path and name to log to, using replacement tokens to rotate based on the date; the default template is "{ApplicationName}-{DateTime:yyyy-MM-dd}.svclog", which rotates on a daily basis. |
| traceOutputOptions | Not used. |

## Path Template Parameters

| Parameter | Description |
| --------- | ----------- |
| {ApplicationName} | Name of the current executable, without the extension. |
| {DateTime} | DateTimeOffset of the log event, in the UTC (+0) timezone. |
| {LocalDateTime} | DateTimeOffset of the log event, in the local timezone. |
| {MachineName} | Local computer name, from Environment.MachineName. |
| {ProcessId} | Id of the current process, from Process.GetCurrentProcess(). |
| {ProcessName} | Name of the current process, from Process.GetCurrentProcess(). |

## Example Config

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.diagnostics>
    <sharedListeners>
      <add name="rollingxml"
        type="Essential.Diagnostics.RollingXmlTraceListener, Essential.Diagnostics.RollingXmlTraceListener"
        initializeData="{ApplicationName}-{DateTime:yyyyMMdd}.svclog" />
    </sharedListeners>
    <sources>
      <source name="ExampleSource" switchValue="All">
        <listeners>
          <clear />
          <add name="rollingxml" />
        </listeners>
      </source>
    </sources>
  </system.diagnostics>
</configuration>
```

## Example Output

Output files from multiple processes can be opened in the Service Trace Viewer tool and correlated across boundaries:

![RollingXmlTraceListener Example Output](../images/RollingXmlTraceListener_TraceViewerExample800.png)

## Config Template

```xml
<add name="rollingxml"
  type="Essential.Diagnostics.RollingXmlTraceListener, Essential.Diagnostics.RollingXmlTraceListener"
  initializeData="{ApplicationName}-{DateTime:yyyy-MM-dd}.svclog"
/>
```
