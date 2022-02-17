# detections_list
detections
https://github.com/elastic/detection-rules
https://github.com/SigmaHQ/sigma
https://github.com/chronicle/detection-rules
https://research.splunk.com/detections/
https://github.com/FalconForceTeam/FalconFriday
https://github.com/Yara-Rules/rules
https://github.com/microsoft/Microsoft-365-Defender-Hunting-Queries
https://github.com/panther-labs/panther-analysis
https://github.com/mitre-attack/car
https://github.com/OTRF
https://github.com/dnif/content

# Run VR for Dead disk forensics
- https://github.com/Velocidex/velociraptor/pull/1564
- can be used to pull artifacts from dead disk
- Steps
  - have Velociraptor analyse the drive and produce a configuration
file.
```
    Will generate a config file - current this automatically:

    Parses the partition table and find the windows OS partition

    Mounts the partition on C: using the ntfs parser - this will be
    available both using the "ntfs" accessor and "file" accessor in VQL

    Some initial registry hives are also mapped using the raw registry
    accessor. In VQL this is available using the "registry" accessor.

    Permissions are adjusted to prevent analysis leaking onto the
    analysis host.

    The info() plugin is mocked to impersonate a windows machine - this
    is required to pass precondition checks in most artifacts.
```
  - Run Artifact


### Generate Configuration file for that dead disk
```
# provide full path
.\velociraptor-v0.6.4-dev-windows-386.exe deaddisk --add_windows_disk "C:\Users\callu\_mystuff\myvirtualbox\VBOX_SHARE\win10homain.raw" ./out.config3.yaml

# linux
./velociraptor-v0.6.4-dev-linux-amd64 deaddisk --add_windows_disk /mnt/c/Users/callu/_mystuff/myvirtualbox/VBOX_SHARE/win10homain.raw ./out.linuxconfig.yaml
```

### Run VR artifact against dead disk
```
# Get prefetch on linux or windows !
.\velociraptor-v0.6.4-dev-windows-386.exe --config .\out.config3.yaml -v artifacts collect Windows.Forensics.Prefetch
 ./velociraptor-v0.6.4-dev-linux-amd64 --config ./out.linuxconfig.yaml artifacts collect Windows.Forensics.Prefetch

.\velociraptor-v0.6.4-dev-windows-386.exe --config .\out.config3.yaml -v artifacts collect Windows.Carving.USNFiles

.\velociraptor-v0.6.4-dev-windows-386.exe --config .\out.config3.yaml -v artifacts collect Generic.Forensic.Timeline

.\velociraptor-v0.6.4-dev-windows-386.exe --config .\out.config3.yaml -v artifacts collect Windows.Timeline.MFT --output mfttimeline.zip

.\velociraptor-v0.6.4-dev-windows-386.exe --config .\out.config3.yaml -v artifacts collect Windows.EventLogs.CondensedAccountUsage --definitions=./custom_artifacts

```


```
# remap artifact definitions to use custom ones in this directory
--definitions=DEFINITIONS
```
