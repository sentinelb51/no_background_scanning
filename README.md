# What is Wi-Fi background scanning
Your network adapter periodically performs a scan to discover available Wi-Fi networks in the background. This behaviour can occur when the adapter is not connected, or even while connected to a network.

## Background scan behaviour
Scans are performed periodically, but their interval may be influenced by various factors, such as network traffic, signal strength, and adapter settings. The adapter may also perform scans when signal strength drops below a certain threshold.

When the adapter detects an available network, it may send a beacon request to the network to discover its capabilities and RSSI (Received Signal Strength Indicator), and will add the network to a list of available networks.

## When this becomes problematic
While network discovery is a fundamental feature of network adapters, sometimes background scanning is wasteful on system resources, for instance when the adapter has a good RSSI, or when there's a limited amount of movement or access points.

# What this fix does
There are 4 registry keys that can influence the network adapter's behaviour on Windows, located in `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Class\{4d36e972-e325-11ce-bfc1-08002be10318}`:

- ScanDisableOnLowTraffic
- ScanDisableOnMediumTraffic
- ScanDisableOnHighOrMulticast
- ScanDisableOnLowLatencyOrQos

There may be more registry keys available under multiple ControlSet folders. The exact location may vary depending on the system. However, modifying keys in one ControlSet folder will usually be reflected in all ControlSet folders.

The registry file sets all of those values to 1.

## What the values mean
Each key determines whether the wireless adapter should disable scanning when there is low, medium, high network traffic, or when there is low latency or [Quality of Service](https://en.wikipedia.org/wiki/Quality_of_service) (QoS) present. QoS is a networking concept that allows prioritisation of certain types of network traffic over others, for instance voice, video, or game data.

### Acceptable values
1 to disable scanning, 0 to enable (default)

## Compromises
It is advisable to do experimenting with these values to fit your environment. Setting all of these to 1 will prioritise network performance over the adapter's ability to detect available networks. Conversely, since it limits the network adapter's ability to find better access points (AP), network performance may be negatively affected when there is a less congested AP available.
