# Log Levels 
- FATAL => 0 => Application is broken no longer working.
- ERROR => 1 => Application is broken but can still run.
- WARN  => 2 => Application is running but maybe broken if not fixed.
- INFO  => 3 => Application is running and everything is fine just for information like application started, database transaction start or end.
- DEBUG => 4 => Application is running and everything is fine but for debugging purpose should not be in production.
- TRACE => 5 => Application is running and everything is fine but for tracing purpose should not be in production.
                But we can use opentelemetry for tracing in production instead of raw trace logs.
- SILLY => 6 => Application is running and everything anything for debugging purpose should not be in production.



