

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog autoReload="true" throwExceptions="true"
      xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <variable name="LogDir" value="${basedir}/locallogs"/>
  <variable name="LogDay" value="${date:format=yyyyMMdd}"/>
  <variable name="jLayout" value='{ "date":"${longdate}","level":"${level}","message":${message}}' />
  <targets>
    <!--layout="${json-encode} ${message}" -->
    <target name="allfile"
            xsi:type="File"
            fileName="${LogDir}/${LogDay}.log"
            encoding="utf-8"
            maxArchiveFiles="100"
            archiveEvery="Day"
            archiveNumbering="DateAndSequence"
            archiveAboveSize="52428800"
            archiveFileName="${LogDir}/{#}.a"
            layout="${jLayout}"
            />
      <!--<layout xsi:type="JsonLayout" includeAllProperties="true" excludeProperties="Comma-separated list (string)">
        <attribute name="time" layout="${longdate}" />
        <attribute name="level" layout="${level:upperCase=true}"/>
        <attribute name="message" layout="${json-encode} ${message}" />
      </layout>
    </target>-->
  </targets>
  <rules>
      <!--All logs, including from Microsoft-->
      <logger name="*" minlevel="DEBUG" writeTo="allfile" />
    </rules>
</nlog>
```