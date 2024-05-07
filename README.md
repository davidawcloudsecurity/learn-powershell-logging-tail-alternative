# learn-powershell-logging-tail-alternative
how to do tail for powershell

Get list of events via Powershell
```ruby
Get-WinEvent -ListLog *
```
Show events via Powershell every 5s
```ruby
# Function to continuously monitor Windows Event Logs
function Monitor-WinEventLogs {
    param (
        [string]$logName = "System",  # Specify the name of the event log (default is System)
        [int]$pollingIntervalSeconds = 5  # Specify the polling interval in seconds (default is 5 seconds)
    )

    # Continuously monitor the event log
    while ($true) {
        # Retrieve new events from the specified event log
        $events = Get-WinEvent -LogName $logName -MaxEvents 1

        # Output new events to the console
        foreach ($event in $events) {
            Write-Host "New event detected in $logName log:"
            Write-Host "Event ID: $($event.Id)"
            Write-Host "Source: $($event.ProviderName)"
            Write-Host "Time: $($event.TimeCreated)"
            Write-Host "Message: $($event.Message)"
            Write-Host "---------------------------------------"
        }

        # Wait for the specified polling interval before checking for new events again
        Start-Sleep -Seconds $pollingIntervalSeconds
    }
}

# Example usage: Monitor the System event log with a polling interval of 5 seconds
Monitor-WinEventLogs -logName "Application" -pollingIntervalSeconds 5
```
Replace "Security" with the name of the event log you want to monitor. If you're unsure about the available event log names, you can use the Get-WinEvent -ListLog * command to list all available event logs on the system.
