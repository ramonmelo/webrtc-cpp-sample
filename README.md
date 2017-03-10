# WebRTC C++ sample

Sample program for using WebRTC(DataChannel) on C++.

# Requirement

* Tested on Ubuntu Linux (16.04)
* WebRTC (https://webrtc.org/native-code/development/). Checkout the master branch.
* Modified version of [PicoJson library](https://github.com/kazuho/picojson/tree/25fc213cca61ea22b3c2e4db8def9927562ba5f7)

# How to Build

1. Clone this repository into your WebRTC build folder:
```bash
git clone git@github.com:ramonmelo/webrtc-cpp-sample.git
```
2. Create a new build using ```gn```:
```bash
# inside the src folder
gn gen out/my_build

# configure the build
gn args out/my_build
# the arguments editor will run, type the configurations below:
```
> symbol_level = 0 \
is_debug = false \
rtc_include_tests = false \
target_os = "linux" \
target_cpu = "x64" \
is_clang = false \
use_sysroot = false \
is_component_build = false \
treat_warnings_as_errors = false \
enable_nacl = false

3. Modify the root BUILD.gn to include our code:
```bash
# open your best editor
vim BUILD.gn # webrtc/src/BUILD.gn

# inside the default group insert
group("default") {
  deps = [
    ...
    "//webrtc-cpp-sample:webrtc_sample",
    ...
  ]
}
```
4. Build all:
```bash
gn gen out/my_build
ninja -C out/my_build
```
5. Wait until all build
6. Execute the example:
```bash
./out/my_build/webrtc_sample
```

# Run

This sample use two consoles to try interprocess communication by WebRTC.
It maybe cannot communicate over NAT each other, because it does not use ICE server.

## Connection

memo : On this sample, Some commands requireing parameter need line of only a semicolon after parameter.

At CONSOLE-1.

```sh
$ cd <path to work>
$ ./sample
0x7fff791c9000:Main thread
0x700000081000:RTC thread
sdp1
0x700000081000:PeerConnectionObserver::RenegotiationNeeded
0x700000081000:CreateSessionDescriptionObserver::OnSuccess
0x700000081000:PeerConnectionObserver::SignalingChange(1)
Offer SDP:begin
<Copy displayed string to the clipboard as STRING-A.>
Offer SDP:end
0x700000081000:SetSessionDescriptionObserver::OnSuccess
0x700000081000:PeerConnectionObserver::IceGatheringChange(1)
0x700000081000:PeerConnectionObserver::IceCandidate
0x700000081000:PeerConnectionObserver::IceCandidate
0x700000081000:PeerConnectionObserver::IceCandidate
0x700000081000:PeerConnectionObserver::IceGatheringChange(2)
sdp3
<Paste STRING-B that it displayed on CONSOLE-2.>
;
0x700000081000:PeerConnectionObserver::SignalingChange(0)
0x700000081000:PeerConnectionObserver::IceConnectionChange(1)
0x700000081000:SetSessionDescriptionObserver::OnSuccess
ice1
<Copy displayed string to the clipboard as STRING-C.>
0x700000081000:PeerConnectionObserver::IceConnectionChange(2)
0x700000081000:PeerConnectionObserver::IceConnectionChange(3)
0x700000081000:DataChannelObserver::StateChange
0x700000081000:PeerConnectionObserver::DataChannel(0x7fd8cb608750, 0x7fd8cb71bef0)
ice2
<Paste STRING-D that it displayed on CONSOLE-2.>
;
```

At CONSOLE-2.

```sh
$ cd <path to work>
$ ./sample
0x7fff791c9000:Main thread
0x700000081000:RTC thread
sdp2
<Paste STRING-A that it displayed on CONSOLE-1.>
;
0x700000081000:PeerConnectionObserver::RenegotiationNeeded
0x700000081000:PeerConnectionObserver::SignalingChange(3)
0x700000081000:SetSessionDescriptionObserver::OnSuccess
0x700000081000:CreateSessionDescriptionObserver::OnSuccess
0x700000081000:PeerConnectionObserver::SignalingChange(0)
Answer SDP:begin
<Copy displayed string to the clipboard as STRING-B.>
Answer SDP:end
0x700000081000:SetSessionDescriptionObserver::OnSuccess
0x700000081000:PeerConnectionObserver::IceGatheringChange(1)
0x700000081000:PeerConnectionObserver::IceCandidate
0x700000081000:PeerConnectionObserver::IceCandidate
0x700000081000:PeerConnectionObserver::IceCandidate
0x700000081000:PeerConnectionObserver::IceGatheringChange(2)
ice2
<Paste STRING-C that it displayed on CONSOLE-1.>
;
0x700000081000:PeerConnectionObserver::IceConnectionChange(1)
0x700000081000:PeerConnectionObserver::IceConnectionChange(2)
0x700000081000:DataChannelObserver::StateChange
0x700000081000:PeerConnectionObserver::DataChannel(0x7fa739e0c0d0, 0x7fa739e08b80)
ice1
<Copy displayed string to the clipboard as STRING-D.>
```

## Send message

You can send messages, after connection is enabled.

```
send
Hello world.
;
```

## Quit

You can watch sequence of quit by typing of "quit".

```
quit
```

EOD
