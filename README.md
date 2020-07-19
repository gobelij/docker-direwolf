# docker-direwolf
A multi-platform image for running Direwolf for APRS projects

## Environment Variables

| Variable    | Required? | Description |
|-------------|-----------|-------------|
| `CALLSIGN`  | Yes | Your callsign & SSID, example `N0CALL-10` |
| `PASSCODE`  | Yes | Passcode for igate login, [find passcode here] |
| `LATITUDE`  | Yes | Latitude for PBEACON, example `42^37.14N` |
| `LONGITUDE` | Yes | Longitude for PBEACON, example `071^20.83W` |
| `COMMENT`   | No  | Override PBEACON default comment |
| `SYMBOL`    | No  | APRS symbol for PBEACON, defaults to `igate` |
| `IGSERVER`  | No  | Override with the APRS server for your region, default for North America `noam.aprs2.net` |
| `ADEVICE`   | No  | Override Direwolf's ADEVICE for digipeater, default is `stdin null` for receive-only igate |
| `FREQUENCY` | No  | Override `rtl_fm` input frequency, default `144.39M` North America APRS |

## Example Usage

### Kubernetes
Example pod definition to run on a [k3s] node with a RTL-SDR Blog v3 dongle plugged into one of the USB ports on a Raspberry Pi 4

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: aprs-igate
  namespace: ham-radio
spec:
  nodeName: homelab-pi4b-node-attic-2
  containers:
  - name: direwolf
    image: w2bro/direwolf
    securityContext:
      privileged: true
    env:
    - name: CALLSIGN
      value: N0CALL-10
    - name: PASSCODE
      value: XXXXX
    - name: FREQUENCY
      value: 144.39M
    - name: LATITUDE
      value: 42^37.14N
    - name: LONGITUDE
      value: 071^20.83W
    volumeMounts:
    - mountPath: /dev/bus/usb/001/004
      name: rtl
  volumes:
  - name: rtl
    hostPath:
      path: /dev/bus/usb/001/004
```

[find passcode here]: http://apps.magicbug.co.uk/passcode/
[k3s]: https://k3s.io