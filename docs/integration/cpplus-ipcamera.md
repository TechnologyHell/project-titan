# CP Plus E25A Camera Investigation & Stream Extraction

## Objective

Integrate the CP Plus E25A IP Camera into TITAN surveillance infrastructure through Home Assistant and go2rtc.

Initial goal was to obtain a live stream accessible through the centralized camera dashboard.

---

## Initial Discovery

Camera was added successfully to Home Assistant using ONVIF auto-discovery.

Configuration:

* Protocol: ONVIF
* Camera IP: 192.168.0.203
* ONVIF Port: 8000
* Username: admin
* Password: configured during setup

Home Assistant was able to display a live feed and control PTZ functions.

This confirmed:

* Network connectivity
* Valid credentials
* Functional ONVIF services
* Available video stream

---

## First Approach - Home Assistant Camera Proxy

Attempted to obtain stream data through Home Assistant camera proxy API.

Method:

* Long-lived access token generation
* Camera proxy endpoint access
* Automated frame capture through curl

Result:

Frames could be retrieved successfully.

However:

* No usable video stream endpoint was exposed.
* Home Assistant only provided snapshot-style access.
* Practical refresh rate was approximately one frame every three seconds.

---

## Snapshot-to-Stream Experiment

Objective:

Convert Home Assistant snapshots into a usable live stream.

Process:

1. Periodically fetch JPEG frame.
2. Save latest frame locally.
3. Attempt conversion to MJPEG stream.
4. Attempt conversion to RTSP stream.
5. Feed resulting stream into go2rtc.

Tools evaluated:

* ffmpeg
* mjpg-streamer
* go2rtc exec streams

Result:

Multiple approaches failed.

Common issues:

* Unsupported source schemes
* Incomplete MJPEG generation
* RTSP publishing failures
* go2rtc timeout errors

Conclusion:

A continuously updated JPEG image is not a practical substitute for a real RTSP camera feed.

---

## Virtual Camera Experiment

Objective:

Present Home Assistant snapshots as a real webcam device.

Components:

* v4l2loopback
* pyfakewebcam
* Python frame injection

Process:

1. Fetch latest JPEG from Home Assistant.
2. Decode image.
3. Inject image into virtual webcam device.
4. Expose virtual webcam as /dev/video10.
5. Import virtual webcam into go2rtc.

Result:

Successful.

Capabilities achieved:

* Camera visible as Linux webcam
* Stream available inside go2rtc
* Browser viewing possible

Limitation:

Refresh rate remained constrained by Home Assistant snapshot delivery.

Observed frame rate:

~0.33 FPS

This solution was considered a functional proof-of-concept rather than a production deployment.

---

## ONVIF Device Manager Investigation

Installed ONVIF Device Manager (ODM).

Purpose:

Identify direct media endpoints exposed by camera.

ODM successfully:

* Authenticated with camera
* Displayed live video
* Enumerated media profiles
* Exposed RTSP configuration

This confirmed that the camera itself was generating a standard RTSP stream.

---

## Breakthrough Discovery

ODM revealed the direct RTSP stream:

rtsp://192.168.0.203:5543/live/channel0

Testing performed using VLC.

Result:

* Smooth video playback
* Stable stream
* Significantly higher frame rate
* Approximate latency of 2 seconds

This eliminated the need for:

* Home Assistant proxy
* Snapshot extraction
* Virtual webcam simulation
* Python frame injection

---

## Final Production Configuration

go2rtc.yaml

streams:
CPPlus_E25a:
- rtsp://admin:<password>@192.168.0.203:5543/live/channel0

Result:

* Native RTSP ingest
* Stable live feed
* Browser access through go2rtc
* Unified surveillance dashboard
* No Home Assistant dependency

---

## Lessons Learned

1. ONVIF integration does not guarantee easy RTSP discovery.

2. Home Assistant camera entities may expose snapshots rather than raw stream endpoints.

3. Snapshot-to-stream conversion is suitable only as a temporary fallback.

4. ONVIF Device Manager is an extremely valuable diagnostic tool.

5. Direct RTSP access should always be preferred when available.

6. A production-grade surveillance system should ingest native RTSP streams whenever possible.
