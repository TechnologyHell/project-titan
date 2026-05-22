# J.A.R.V.I.S.



Just A Rather Very Intelligent System





ACTIVATION OF VENV : source ~/jarvis-venv/bin/activate





**Hardware Configuration**



Platform 	: Raspberry Pi 4 Model B with 4GB RAM

Memory		: 4DB LPDDR4 3200 SDRAM Onboard

Storage		: Sandisk 64GB Class 10 Micro SD Card

Power Supply	: Official Raspberry Pi 5v Type C Adapter

Sound DAC	: AntEsports 7.1 USB Gaming Sound Card

Microphone 	: Maono AU-400 Lavalier Microphone

Speakers	: Boat SpinX Pro







**Software Configuration**



Operating System: Raspberry OS Lite 64bit



#### 



#### Phase 1 : Initiation

This phase includes setting up the basic services and dependencies, on the Pi alone, including TTS, STT and python requirements, to fulfil basics of wake word detection, and response generation.

It is not expected for insane performance and accuracy to be achieved in this phase alone. Rather this is more towards giving JARVIS ears, mouth, and a brain to interact.





###### Part A : System Setup



1. **Folder Structure Setup**

jarvis/

├── audio/

│   ├── mic.py

│   └── speaker.py

├── wakeword/

│   └── wake\_listener.py

├── stt/

│   └── whisper\_stt.py

├── tts/

│   └── piper\_tts.py

├── core/

│   └── main\_loop.py

└── config/

    └── audio.yaml







**2. Lock in audio hardware for JARVIS to remember**

nano jarvis/config/audio.yaml



audio:

  input:

    type: alsa

    card: 3

    device: 0

    samplerate: 16000

    channels: 1



  output:

    type: alsa

    card: 3

    device: 0

    samplerate: 22050

    channels: 1







**3. Lock audio hardware at OS level**

sudo nano /etc/asound.conf



defaults.pcm.card 3

defaults.ctl.card 3



Install PortAudio : sudo apt install -y portaudio19-dev libportaudio2

Install LibaSound2: sudo apt install -y libasound2-dev







**4. Install Python Dependencies (inside the venv)**

Install full Python venv support : sudo apt install -y python3-full python3-venv

Create a venv for JARVIS	 : python3 -m venv ~/jarvis-venv

Activate the venv for JARVIS	 : source ~/jarvis-venv/bin/activate

Install packages inside the venv : pip install --upgrade pip

 				   pip install sounddevice numpy pyyaml







**5. Test if PyVenv is able to detect sound devices**

python - <<EOF

import sounddevice as sd

print(sd.query\_devices())

EOF



Should list all available sound hardware







###### Part B : Make JARVIS able to Hear



**1. Mic listener code (audio/mic.py)**

import sounddevice as sd

import numpy as np



SAMPLE\_RATE = 16000

CHANNELS = 1

DURATION = 5  # seconds



def record\_audio():

    print("🎤 Listening...")

    audio = sd.rec(

        int(DURATION \* SAMPLE\_RATE),

        samplerate=SAMPLE\_RATE,

        channels=CHANNELS,

        dtype=np.int16

    )

    sd.wait()

    print("✅ Audio captured")

    return audio



if \_\_name\_\_ == "\_\_main\_\_":

    record\_audio()





Should output as :
(jarvis-venv) tech@jarvis:~ $ python3 jarvis/audio/mic.py

🎤 Listening...

✅ Audio captured









**2. Wake Word Detection Configuration (wakeword/wake\_listener.py)**

Create Wake Listener : nano jarvis/wakeword/wake\_listener.py



import sounddevice as sd

import numpy as np

from collections import deque



SAMPLE\_RATE = 44100

CHANNELS = 2

FRAME\_SIZE = 2048

THRESHOLD = 0.02

RECORD\_SECONDS = 2.5

PREROLL\_SECONDS = 0.5  # keep last 500ms



def rms(audio):

    return np.sqrt(np.mean(audio\*\*2))



def listen():

    print("🎧 Jarvis idle... waiting for speech")



    preroll = deque(

        maxlen=int((SAMPLE\_RATE \* PREROLL\_SECONDS) / FRAME\_SIZE)

    )



    recorded\_frames = \[]



    with sd.InputStream(

        samplerate=SAMPLE\_RATE,

        channels=CHANNELS,

        dtype="float32",

        blocksize=FRAME\_SIZE,

    ) as stream:

        while True:

            audio, \_ = stream.read(FRAME\_SIZE)

            preroll.append(audio)



            if rms(audio) > THRESHOLD:

                print("🟢 Speech detected")

                recorded\_frames.extend(preroll)

                break



        # record rest of phrase

        remaining\_frames = int((RECORD\_SECONDS \* SAMPLE\_RATE) / FRAME\_SIZE)

        for \_ in range(remaining\_frames):

            audio, \_ = stream.read(FRAME\_SIZE)

            recorded\_frames.append(audio)



    full\_audio = np.concatenate(recorded\_frames, axis=0)

    print("✅ Phrase recorded")

    return full\_audio





Expected Output :

python3 jarvis/wakeword/wake\_listener.py

🎧 Jarvis idle... waiting for speech

🟢 Speech detected

🎙 Recording phrase...

✅ Phrase recorded









3\. **Whisper.cpp Speech-to-Text Configuration**



Installation :

sudo apt install -y cmake build-essential

cd ~

git clone https://github.com/ggerganov/whisper.cpp

cd whisper.cpp

cmake -B build

cmake --build build -j4



Verification :
ls build/bin

Should list all folders such as main bench quantize



Fix Whisper Binary Path :

nano jarvis/stt/whisper\_stt.py

->

import os

import subprocess

import tempfile

import soundfile as sf

import numpy as np

from scipy.signal import resample



WHISPER\_BIN = os.path.expanduser("~/whisper.cpp/build/bin/whisper-cli")

MODEL\_PATH = os.path.expanduser("~/whisper.cpp/models/ggml-tiny.en.bin")



TARGET\_SR = 16000





def transcribe(audio, sample\_rate: int) -> str:

    # mono

    if audio.ndim > 1:

        audio = np.mean(audio, axis=1)



    # resample

    if sample\_rate != TARGET\_SR:

        audio = resample(audio, int(len(audio) \* TARGET\_SR / sample\_rate))

        sample\_rate = TARGET\_SR



    audio = audio.astype(np.float32)

    audio /= np.max(np.abs(audio) + 1e-9)



    with tempfile.NamedTemporaryFile(suffix=".wav", delete=False) as f:

        sf.write(f.name, audio, sample\_rate)

        wav\_path = f.name



    try:

        result = subprocess.run(

            \[

                WHISPER\_BIN,

                "-m", MODEL\_PATH,

                "-f", wav\_path,

                "--language", "en",

                "--no-timestamps",

                "--beam-size", "1",

                "--best-of", "1",

                "--threads", "4"

            ],

            stdout=subprocess.PIPE,

            stderr=subprocess.DEVNULL,   # silence timing spam

            text=True

        )



        text = result.stdout.strip().lower()

        return text



    finally:

        os.remove(wav\_path)







Download correct model :

 cd ~/whisper.cpp

./models/download-ggml-model.sh tiny.en



Verification :

ls ~/whisper.cpp/models | grep tiny









**4. Create WakeUp Pipeline**



New File under Jarvis/Core/ :  nano jarvis/core/test\_wake\_pipeline.py



from jarvis.wakeword.wake\_listener import listen

from jarvis.stt.whisper\_stt import transcribe



\# Flexible wake keywords (MCU style)

WAKE\_KEYWORDS = \[

    "jarvis",

    "hey jarvis",

    "wake up",

    "listen",

    "daddy",

    "are you there"

]



def is\_wake\_phrase(text: str) -> bool:

    text = text.lower()

    return any(keyword in text for keyword in WAKE\_KEYWORDS)





print("🧠 Jarvis standing by...")



\# 1. Wait for speech + record phrase

audio = listen()



\# 2. Speech to text

text = transcribe(audio, sample\_rate=44100)



print(f"📝 You said: {text}")



\# 3. Decide wake

if is\_wake\_phrase(text):

    print("🟢 Wake phrase confirmed. Jarvis is awake.")

else:

    print("🔴 Not a wake phrase. Ignoring.")







Ensure Python can import modules

touch jarvis/\_\_init\_\_.py

touch jarvis/audio/\_\_init\_\_.py

touch jarvis/wakeword/\_\_init\_\_.py

touch jarvis/stt/\_\_init\_\_.py

touch jarvis/core/\_\_init\_\_.py







Install Soundfile Dependencies :

sudo apt install -y libsndfile1

pip install soundfile







###### Part C : Make JARVIS able to Speak



**1. Installing TTS Engine for Jarvis (PIPER)**



Installation :

sudo apt install -y sox libespeak-ng1

pip install piper-tts



Verification :

piper --help



Download a GOOD Jarvis like Voice

mkdir -p ~/piper-voices

cd ~/piper-voices

wget https://huggingface.co/rhasspy/piper-voices/resolve/main/en/en\_US/lessac/medium/en\_US-lessac-medium.onnx

wget https://huggingface.co/rhasspy/piper-voices/resolve/main/en/en\_US/lessac/medium/en\_US-lessac-medium.onnx.json









**2. Create Text-To-Speech Module**

nano jarvis/tts/piper\_tts.py



import subprocess

import tempfile

import os



VOICE\_MODEL = os.path.expanduser("~/piper-voices/en\_US-lessac-medium.onnx")

VOICE\_CONFIG = os.path.expanduser("~/piper-voices/en\_US-lessac-medium.onnx.json")





def speak(text: str):

    if not text.strip():

        return



    with tempfile.NamedTemporaryFile(suffix=".wav", delete=False) as f:

        wav\_path = f.name



    try:

        subprocess.run(

            \[

                "piper",

                "--model", VOICE\_MODEL,

                "--config", VOICE\_CONFIG,

                "--output\_file", wav\_path

            ],

            input=text,

            text=True,

            check=True

        )



        subprocess.run(\["aplay", wav\_path], check=True)



    finally:

        os.remove(wav\_path)







Testing the TTS Engine :

python3



from jarvis.tts.piper\_tts import speak

speak("Yes sir. Took you long enough.")







**3. Wire TTS into Wake Pipeline**

nano jarvis/core/test\_wake\_pipeline.py



Import SPEAK Module :

from jarvis.tts.piper\_tts import speak



Add SPEAK to wake presence :

if is\_wake\_phrase(text):

    print("🟢 Wake phrase confirmed. Jarvis is awake.")

    speak("Yes sir. I'm listening.")









#### Phase 1 Conclusion

We designed the system structure, prepared dependencies, setup TTS and STT, and wake word detection to turn on JARVIS.

Results - JARVIS speaks up once it hears the wake command.
Cons - Too much of processing time \& delay before spoken response (~15 seconds each response).





====================================================================================================================================





#### Phase 2 : Cognition





###### Part A : Instant Acknowledgement + Awake Window



1. **Add very fast acknowledgement**

nano jarvis/tts/piper\_tts.py



def ack():

    # ultra-short acknowledgement

    speak("Yes?")





**2. Add awake window state**

nano jarvis/core/test\_wake\_pipeline.py



import time

from jarvis.tts.piper\_tts import speak, ack



AWAKE\_WINDOW\_SECONDS = 8



if is\_wake\_phrase(text):

    print("🟢 Wake phrase confirmed. Jarvis is awake.")

    ack()  # instant response



    awake\_until = time.time() + AWAKE\_WINDOW\_SECONDS

    print(f"🟡 Awake window started ({AWAKE\_WINDOW\_SECONDS}s)")





**3. Testing Acknowledgement**

python3 -m jarvis.core.test\_wake\_pipeline



Result - instant "YES" response after wake word is recognized





###### Part B : Silence based recording Cut-off



1. **Modify wake\_listener.py**

nano jarvis/wakeword/wake\_listener.py



Replace old content with new version:





import sounddevice as sd

import numpy as np

from collections import deque

import time



SAMPLE\_RATE = 44100

CHANNELS = 2

FRAME\_SIZE = 2048



THRESHOLD = 0.02

SILENCE\_THRESHOLD = 0.015

SILENCE\_DURATION = 0.5   # seconds

PREROLL\_SECONDS = 0.4



def rms(audio):

    return np.sqrt(np.mean(audio\*\*2))





def listen():

    print("🎧 Jarvis idle... waiting for speech")



    preroll = deque(

        maxlen=int((SAMPLE\_RATE \* PREROLL\_SECONDS) / FRAME\_SIZE)

    )



    recorded = \[]

    silence\_start = None



    with sd.InputStream(

        samplerate=SAMPLE\_RATE,

        channels=CHANNELS,

        dtype="float32",

        blocksize=FRAME\_SIZE,

    ) as stream:



        # wait for speech

        while True:

            audio, \_ = stream.read(FRAME\_SIZE)

            preroll.append(audio)



            if rms(audio) > THRESHOLD:

                print("🟢 Speech detected")

                recorded.extend(preroll)

                break



        # record until silence

        while True:

            audio, \_ = stream.read(FRAME\_SIZE)

            recorded.append(audio)



            level = rms(audio)



            if level < SILENCE\_THRESHOLD:

                if silence\_start is None:

                    silence\_start = time.time()

                elif time.time() - silence\_start >= SILENCE\_DURATION:

                    break

            else:

                silence\_start = None



    full\_audio = np.concatenate(recorded, axis=0)

    print("✅ Phrase recorded")

    return full\_audio





Test for new recording pattern :

python3 -m jarvis.core.test\_wake\_pipeline









###### PART C : Profiling



1. **Measure Total Round Trip**



Add a tiny profile helper

nano jarvis/core/test\_wake\_pipeline.py



At the top, add:

import time



def now():

    return time.perf\_counter()





Find the start of execution and add:

t0 = now()



After audio = listen() add:

t\_record = now()

print(f"⏱ Recording time: {t\_record - t0:.2f}s")



After text = transcribe(...) add:

t\_stt = now()

print(f"⏱ STT time: {t\_stt - t\_record:.2f}s")



After wake logic (before speaking) add:

t\_logic = now()

print(f"⏱ Logic time: {t\_logic - t\_stt:.2f}s")



After ack() or speak(...) add:

t\_tts = now()

print(f"⏱ TTS time: {t\_tts - t\_logic:.2f}s")

print(f"⏱ TOTAL time: {t\_tts - t0:.2f}s")



Run and collect data :

python3 -m jarvis.core.test\_wake\_pipeline



Result :

(jarvis-venv) tech@jarvis:~ $ python3 -m jarvis.core.test\_wake\_pipeline

🧠 Jarvis standing by...

🎧 Jarvis idle... waiting for speech

🟢 Speech detected

✅ Phrase recorded

⏱ Recording time: 2.26s

⏱ STT time: 6.64s

📝 You said: hey jarvis

🟢 Wake phrase confirmed. Jarvis is awake.

⏱ Logic time: 0.00s

2025-12-26 15:11:58.327604271 \[W:onnxruntime:Default, device\_discovery.cc:164 DiscoverDevicesForPlatform] GPU device discovery failed: device\_discovery.cc:89 ReadFileContents Failed to open file: "/sys/class/drm/card1/device/vendor"

Playing WAVE '/tmp/tmp138xeg\_5.wav' : Signed 16 bit Little Endian, Rate 22050 Hz, Mono

⏱ TTS time: 5.14s

⏱ TOTAL time: 14.04s

🟡 Awake window started (8s)

(jarvis-venv) tech@jarvis:~ $







**2. Record directly at 16kHz \& reduce silence cutoff to 0.3s**

nano jarvis/wakeword/wake\_listener.py



CHANNELS = 1          	# mono

FRAME\_SIZE = 1024     	# smaller frames for faster response

SILENCE\_DURATION = 0.3	# was 0.5





**3. Pre-Recorded Acknowledgement WAVs**



Create WAV generation function :

nano ~/jarvis/tts/piper\_tts.py



\# -----------------------------

\# Export TTS to WAV (no playback)

\# -----------------------------

def export\_wav(text: str, output\_path: str):

    if not text.strip():

        return



    subprocess.run(

        \[

            "piper",

            "--model", VOICE\_MODEL,

            "--config", VOICE\_CONFIG,

            "--output\_file", output\_path

        ],

        input=text,

        text=True,

        check=True

    )







Create ack directory

mkdir -p jarvis/audio/ack



Generate ack WAVs

python3 ->



from jarvis.tts.piper\_tts import export\_wav

import os



ACK\_DIR = os.path.expanduser("~/jarvis/audio/ack")

os.makedirs(ACK\_DIR, exist\_ok=True)



phrases = {

    "yes.wav": "Yes?",

    "listening.wav": "I'm listening.",

    "go\_on.wav": "Go on.",

    "yes\_sir.wav": "Yes sir."

}



for filename, text in phrases.items():

    path = os.path.join(ACK\_DIR, filename)

    print(f"Generating {filename}")

    export\_wav(text, path)







**4. ACK Player on Response for Wake**

nano ~/jarvis/audio/ack\_player.py



import os

import random

import subprocess



ACK\_DIR = os.path.expanduser("~/jarvis/audio/ack")



def play\_ack():

    if not os.path.isdir(ACK\_DIR):

        return



    wavs = \[f for f in os.listdir(ACK\_DIR) if f.endswith(".wav")]

    if not wavs:

        return



    wav = os.path.join(ACK\_DIR, random.choice(wavs))

    subprocess.run(

        \["aplay", wav],

        stdout=subprocess.DEVNULL,

        stderr=subprocess.DEVNULL

    )





**5. Replace TTS Ack in Wake Pipeline**

nano ~/jarvis/core/test\_wake\_pipeline.py



from jarvis.audio.ack\_player import play\_ack



Replace  ack()  with  play\_ack()







**6. Fuzzy wake detection**

Install RapidFuzz :

pip install rapidfuzz



Create Wake Phrase Database

nano ~/jarvis/wakeword/wake\_phrases.py



\# Canonical wake phrases

WAKE\_PHRASES = {

    "hey jarvis": {

        "type": "generic"

    },

    "jarvis": {

        "type": "generic"

    },

    "wake up daddy's home": {

        "type": "custom",

        "response": "welcome\_back.wav"

    },

    "daddy's home": {

        "type": "custom",

        "response": "welcome\_back.wav"

    }

}



\# Common STT mis-hearings (manual expansion)

ALIASES = \[

    "jardwiz",     # <-- ADD THIS

    "jardwis",

    "judmas",

    "john lewis",

    "john miss",

    "jar vis",

    "jarviz",

    "jar vish",

    "jarwis",

    "hey john lewis",

    "hey judmas"

]







Fuzzy Wake Detector Logic :

nano ~/jarvis/wakeword/wake\_detector.py





import jellyfish



WAKE\_PREFIXES = \[

    "hey",

    "ok",

    "okay",

    "yo",

    "sup"

]



JARVIS\_ALIASES = \[

    "jarvis",

    "jardwiz",

    "jardwins",

    "jarwis",

    "jarwiz",

    "jervis",

    "jealous",

    "nervous",

    "john lewis",

    "john miss",

    "jardin",

    "jardins"

]





def normalize(text: str) -> str:

    return (

        text.lower()

        .replace(",", "")

        .replace(".", "")

        .replace("?", "")

        .strip()

    )





def sounds\_like\_jarvis(word: str) -> bool:

    return jellyfish.metaphone(word) == jellyfish.metaphone("jarvis")





def detect\_wake(text: str):

    text = normalize(text)

    words = text.split()



    # 1️⃣ Prefix + alias (HEY / OK / YO + jarvis-like)

    if any(p in words for p in WAKE\_PREFIXES):

        for alias in JARVIS\_ALIASES:

            if alias in text:

                return {"type": "generic"}



        for w in words:

            if sounds\_like\_jarvis(w):

                return {"type": "generic"}



    # 2️⃣ Alias alone (jarvis / jealous / nervous etc.)

    for alias in JARVIS\_ALIASES:

        if alias == text or alias in words:

            return {"type": "generic"}



    # 3️⃣ Phonetic single-word fallback

    if len(words) == 1 and sounds\_like\_jarvis(words\[0]):

        return {"type": "generic"}



    return None









Result :

Works when JARVIS is spoken correctly, often mishears what is said.



Solution :
Shifting to PicoVoice Porcupine







###### Part D : Picovoice Wake Word Detection ( Porcupine )



1. **Create a free Picovoice Account**

-> Start Building : Create Wake Word

select Language ;

define WakeWord ;

record WakeWord ;

Train model ;

download .ppn file



Download link generated

<confidential>



**2. Get the downloaded PPN file on Pi**

Unzip the file : unzip jarvis.zip

Rename the PPN : mv \*.ppn jarvis.ppn



**3. Install Porcupine Runtime (Pi) \& PyAudio**

pip install pvporcupine

sudo apt install -y portaudio19-dev

pip install pyaudio





**4. Minimal Wake Test Script**

nano ~/jarvis/wakeword/pv\_wake\_test.py  MUST REPLACE "ACCESS KEY"



import pvporcupine

import pyaudio

import struct

import os



ACCESS\_KEY = "PASTE\_YOUR\_PICOVOICE\_ACCESS\_KEY\_HERE"



KEYWORD\_PATH = os.path.expanduser(

    "~/jarvis/wakeword/models/jarvis.ppn"

)



porcupine = pvporcupine.create(

    access\_key=ACCESS\_KEY,

    keyword\_paths=\[KEYWORD\_PATH],

    sensitivities=\[0.6]   # we’ll tune later

)



pa = pyaudio.PyAudio()



stream = pa.open(

    rate=porcupine.sample\_rate,

    channels=1,

    format=pyaudio.paInt16,

    input=True,

    frames\_per\_buffer=porcupine.frame\_length

)



print("🎧 Listening for 'Jarvis'... (Ctrl+C to stop)")



try:

    while True:

        pcm = stream.read(

            porcupine.frame\_length,

            exception\_on\_overflow=False

        )

        pcm = struct.unpack\_from(

            "h" \* porcupine.frame\_length, pcm

        )



        result = porcupine.process(pcm)

        if result >= 0:

            print("🟢 Wake word detected!")

except KeyboardInterrupt:

    print("\\nStopping...")

finally:

    stream.close()

    pa.terminate()

    porcupine.delete()







**5. Creating a Reusable Porcupine listener**

nano ~/jarvis/wakeword/pv\_wake\_listener.py



import pvporcupine

import pyaudio

import struct

import os





class PorcupineWakeListener:

    def \_\_init\_\_(self, access\_key, keyword\_path, sensitivity=0.8):

        self.porcupine = pvporcupine.create(

            access\_key=access\_key,

            keyword\_paths=\[keyword\_path],

            sensitivities=\[sensitivity]

        )



        self.pa = pyaudio.PyAudio()

        self.stream = self.pa.open(

            rate=self.porcupine.sample\_rate,

            channels=1,

            format=pyaudio.paInt16,

            input=True,

            frames\_per\_buffer=self.porcupine.frame\_length

        )



    def wait\_for\_wake(self):

        while True:

            pcm = self.stream.read(

                self.porcupine.frame\_length,

                exception\_on\_overflow=False

            )

            pcm = struct.unpack\_from(

                "h" \* self.porcupine.frame\_length, pcm

            )



            if self.porcupine.process(pcm) >= 0:

                return True



    def close(self):

        self.stream.close()

        self.pa.terminate()

        self.porcupine.delete()









**6. Integrating into the Main Loop**

nano ~/jarvis/core/main\_loop.py



import time

import random

import os



from jarvis.wakeword.pv\_wake\_listener import PorcupineWakeListener

from jarvis.audio.mic import record\_command

from jarvis.audio.ack\_player import play\_ack

from jarvis.stt.whisper\_stt import transcribe

from jarvis.tts.piper\_tts import speak





ACCESS\_KEY = "YOUR\_PICOVOICE\_ACCESS\_KEY"

KEYWORD\_PATH = os.path.expanduser(

    "~/jarvis/wakeword/models/jarvis.ppn"

)



ACK\_DIR = os.path.expanduser("~/jarvis/audio/ack")





def random\_ack():

    files = \[f for f in os.listdir(ACK\_DIR) if f.endswith(".wav")]

    return os.path.join(ACK\_DIR, random.choice(files))





def main():

    print("🧠 Jarvis online. Awaiting wake word.")



    wake\_listener = PorcupineWakeListener(

        access\_key=ACCESS\_KEY,

        keyword\_path=KEYWORD\_PATH,

        sensitivity=0.8

    )



    while True:

        # 1️⃣ WAIT FOR WAKE (INSTANT)

        wake\_listener.wait\_for\_wake()



        # 2️⃣ ACK IMMEDIATELY

        play\_ack()



        # 3️⃣ RECORD COMMAND (≈2 seconds)

        audio\_path = record\_command(

            max\_seconds=2.0,

            silence\_timeout=0.3

        )



        # 4️⃣ RUN STT ONCE

        text = transcribe(audio\_path)

        print(f"📝 Command: {text}")



        if not text.strip():

            continue



        # 5️⃣ SIMPLE LOGIC (placeholder)

        if "time" in text:

            speak(time.strftime("It is %I:%M %p"))

        else:

            speak("I heard you.")





if \_\_name\_\_ == "\_\_main\_\_":

    main()









**7. Update Mic Recording (upto 2 seconds)**

nano jarvis/audio/mic.py



def record\_command(max\_seconds=2.0, silence\_timeout=0.3):

    """

    Records a short command after wake.

    """

    # Your existing implementation is fine

    # Just ensure max\_seconds is honored

    pass







nano jarvis/stt/whisper\_stt.py  -> Replace entire file



import os

import subprocess



WHISPER\_BIN = os.path.expanduser("~/whisper.cpp/build/bin/whisper-cli")

MODEL\_PATH = os.path.expanduser("~/whisper.cpp/models/ggml-tiny.en.bin")





def transcribe(wav\_path: str) -> str:

    """

    Transcribe a WAV file using whisper.cpp.

    Expects a valid WAV file path.

    """



    if not os.path.exists(wav\_path):

        return ""



    result = subprocess.run(

        \[

            WHISPER\_BIN,

            "-m", MODEL\_PATH,

            "-f", wav\_path,

            "--language", "en",

            "--no-timestamps",

            "--beam-size", "1",

            "--best-of", "1",

            "--threads", "4"

        ],

        stdout=subprocess.PIPE,

        stderr=subprocess.DEVNULL,   # silence timing spam

        text=True

    )



    return result.stdout.strip().lower()









nano jarvis/core/main\_loop.py

-> Replace the main function :

def main():

    print("🧠 Jarvis online. Awaiting wake word.")



    wake\_listener = PorcupineWakeListener(

        access\_key=ACCESS\_KEY,

        keyword\_path=KEYWORD\_PATH,

        sensitivity=0.8

    )



    try:

        while True:

            # 1️⃣ WAIT FOR WAKE (INSTANT)

            wake\_listener.wait\_for\_wake()



            # 2️⃣ ACK IMMEDIATELY

            play\_ack()



            # 3️⃣ RECORD COMMAND (≈2 seconds)

            audio\_path = record\_command(

                max\_seconds=2.0,

                silence\_timeout=0.3

            )



            if not audio\_path:

                continue



            # 4️⃣ TRANSCRIBE

            text = transcribe(audio\_path)

            print(f"📝 Command: {text}")



            if not text.strip():

                continue



            # 5️⃣ TEMP RESPONSE

            speak("I heard you.")



    except KeyboardInterrupt:

        print("\\n🛑 Shutting down Jarvis...")



    finally:

        wake\_listener.close()

        print("✅ Audio resources released. Goodbye.")



if \_\_name\_\_ == "\_\_main\_\_":

    main()







#### Phase 2 Conclusion

We optimized the entire architecture for better and faster acknowledgement upon calling of the wake word.
Initially working on the STT engine, for detection, conversion, and acknowledgement via pre-recorded WAVs instead of calling the TTS engine, massively cutting down on processing time requirements.

Shifted to Picovoice Porcupine for wake word detection and constant looping works, detecting multiple wake-up calls, and responding to each, with the pre-generated acknowledgement.





====================================================================================================================================





#### Optional : Samba Setup



1. **Installation of Samba**

sudo apt update

sudo apt install -y samba samba-common-bin



**2. Set SAMBA password**

sudo smbpasswd -a tech

Enable the user : sudo smbpasswd -e tech



**3. Configure the Samba configuration File**

\[jarvis]

   path = /home/tech/jarvis

   valid users = tech

   read only = no

   browsable = yes

   writable = yes

   create mask = 0775

   directory mask = 0775



**4. Fix folder permissions**

Owns the folder : sudo chown -R tech:tech /home/tech

Group write perm: chmod -R 775 /home/tech



**5. Restart Samba**

sudo systemctl restart smbd

sudo systemctl restart nmbd



**6. Map Network Drive**

\\\\192.168.0.6\\tech





====================================================================================================================================





#### Extras : Stability Fixes



1. **Make the VENV get activated as soon as the Pi turns On**

nano ~/.bashrc



\# Auto-activate JARVIS venv

if \[ -d "$HOME/jarvis-venv" ]; then

&nbsp;   source "$HOME/jarvis-venv/bin/activate"

fi





**2. Store the Picovoice Access Key as Environment Variable**

export PICOVOICE\_ACCESS\_KEY="your\_real\_key\_here"

export ACCESS\_KEY="your\_real\_key\_here"







**SO FAR SO STABLE VERSION :**



**#### wake\_listener.py**



import pvporcupine

import pyaudio

import numpy as np

import scipy.signal





class PorcupineWakeListener:

&nbsp;   def \_\_init\_\_(self, access\_key, keyword\_path, sensitivity=0.98):

&nbsp;       self.porcupine = pvporcupine.create(

&nbsp;           access\_key=access\_key,

&nbsp;           keyword\_paths=\[keyword\_path],

&nbsp;           sensitivities=\[sensitivity]

&nbsp;       )



&nbsp;       self.pa = pyaudio.PyAudio()



&nbsp;       # 🔒 HARD LOCK to known working ALSA device

&nbsp;       self.device\_index = None

&nbsp;       self.device\_rate = 48000  # USB DAC native rate

&nbsp;       self.target\_rate = self.porcupine.sample\_rate



&nbsp;       # Find device index that corresponds to hw:3,0

&nbsp;       for i in range(self.pa.get\_device\_count()):

&nbsp;           info = self.pa.get\_device\_info\_by\_index(i)

&nbsp;           if (

&nbsp;               info\["maxInputChannels"] > 0

&nbsp;               and "usb" in info\["name"].lower()

&nbsp;           ):

&nbsp;               self.device\_index = i

&nbsp;               break



&nbsp;       if self.device\_index is None:

&nbsp;           raise RuntimeError(

&nbsp;               "PyAudio cannot see USB mic. ALSA-only device detected."

&nbsp;           )



&nbsp;       self.device\_frame\_len = int(

&nbsp;           self.device\_rate / self.target\_rate

&nbsp;       ) \* self.porcupine.frame\_length



&nbsp;       self.stream = self.pa.open(

&nbsp;           rate=self.device\_rate,

&nbsp;           channels=1,

&nbsp;           format=pyaudio.paInt16,

&nbsp;           input=True,

&nbsp;           input\_device\_index=self.device\_index,

&nbsp;           frames\_per\_buffer=self.device\_frame\_len

&nbsp;       )



&nbsp;       print(

&nbsp;           f"🎙 Using mic via PyAudio index {self.device\_index} "

&nbsp;           f"(48000 Hz → {self.target\_rate} Hz)"

&nbsp;       )



&nbsp;   def wait\_for\_wake(self):

&nbsp;       while True:

&nbsp;           raw = self.stream.read(

&nbsp;               self.device\_frame\_len,

&nbsp;               exception\_on\_overflow=False

&nbsp;           )



&nbsp;           pcm = np.frombuffer(raw, dtype=np.int16)



&nbsp;           pcm16 = scipy.signal.resample\_poly(

&nbsp;               pcm,

&nbsp;               up=1,

&nbsp;               down=int(self.device\_rate / self.target\_rate)

&nbsp;           ).astype(np.int16)



&nbsp;           if self.porcupine.process(pcm16.tolist()) >= 0:

&nbsp;               return True



&nbsp;   def close(self):

&nbsp;       self.stream.close()

&nbsp;       self.pa.terminate()

&nbsp;       self.porcupine.delete()





**#### main\_loop.py**



import time

import random

import os



from jarvis.wakeword.pv\_wake\_listener import PorcupineWakeListener

from jarvis.audio.mic import record\_command

from jarvis.audio.ack\_player import play\_ack

from jarvis.stt.whisper\_stt import transcribe

from jarvis.tts.piper\_tts import speak



ACCESS\_KEY="E2N/iS3toDAjAsdeJf9YfIpZAmVNC6cxrXTpV0K+DOlguNk2spPkuA=="



KEYWORD\_PATH = os.path.expanduser(

&nbsp;   "~/jarvis/wakeword/models/jarvis.ppn"

)



ACK\_DIR = os.path.expanduser("~/jarvis/audio/ack")





def random\_ack():

&nbsp;   files = \[f for f in os.listdir(ACK\_DIR) if f.endswith(".wav")]

&nbsp;   return os.path.join(ACK\_DIR, random.choice(files))



def main():

&nbsp;   print("🧠 Jarvis online. Awaiting wake word.")



&nbsp;   print("Creating wake listener")



&nbsp;   wake\_listener = PorcupineWakeListener(

&nbsp;       access\_key=ACCESS\_KEY,

&nbsp;       keyword\_path=KEYWORD\_PATH,

&nbsp;       sensitivity=0.98

&nbsp;   )



&nbsp;   print("Waiting for wake word")



&nbsp;   try:

&nbsp;       while True:

&nbsp;           # 1️⃣ WAIT FOR WAKE (INSTANT)

&nbsp;           wake\_listener.wait\_for\_wake()

&nbsp;           print("Wake word detected")



&nbsp;           # 2️⃣ ACK IMMEDIATELY

&nbsp;           play\_ack()



&nbsp;           # 3️⃣ RECORD COMMAND (≈2 seconds)

&nbsp;           audio\_path = record\_command(

&nbsp;               max\_seconds=2.0,

&nbsp;               silence\_timeout=0.3

&nbsp;           )



&nbsp;           if not audio\_path:

&nbsp;               continue



&nbsp;           # 4️⃣ TRANSCRIBE

&nbsp;           text = transcribe(audio\_path)

&nbsp;           print(f"📝 Command: {text}")



&nbsp;           if not text.strip():

&nbsp;               continue



&nbsp;           # 5️⃣ TEMP RESPONSE

&nbsp;           speak("I heard you.")



&nbsp;   except KeyboardInterrupt:

&nbsp;       print("\\n🛑 Shutting down Jarvis...")



&nbsp;   finally:

&nbsp;       wake\_listener.close()

&nbsp;       print("✅ Audio resources released. Goodbye.")



if \_\_name\_\_ == "\_\_main\_\_":

&nbsp;   main()





====================================================================================================================================





#### Phase 3 : Server Integration



1. **Prep the server with Prerequisites**

sudo apt update

sudo apt install -y \\

&nbsp; build-essential cmake git python3 python3-venv \\

&nbsp; ffmpeg sox libsndfile1





**2. Create Backend VENV**

Create : python3 -m venv ~/jarvis-backend-venv

Activate: source ~/jarvis-backend-venv/bin/activate

Install Flask : pip install flask





**3. Install Whisper.cpp** 

cd ~

git clone https://github.com/ggerganov/whisper.cpp

cd whisper.cpp

cmake -B build

cmake --build build -j$(nproc)

./models/download-ggml-model.sh small.en





**4. Create STT API on TITAN**

nano ~/stt\_server.py



from flask import Flask, request, jsonify

import subprocess

import tempfile

import os



app = Flask(\_\_name\_\_)



WHISPER\_BIN = os.path.expanduser("~/whisper.cpp/build/bin/whisper-cli")

MODEL\_PATH = os.path.expanduser("~/whisper.cpp/models/ggml-small.en.bin")



@app.route("/stt", methods=\["POST"])

def stt():

&nbsp;   if "audio" not in request.files:

&nbsp;       return jsonify({"error": "no audio"}), 400



&nbsp;   audio = request.files\["audio"]



&nbsp;   with tempfile.NamedTemporaryFile(suffix=".wav", delete=False) as f:

&nbsp;       audio.save(f.name)

&nbsp;       wav\_path = f.name



&nbsp;   try:

&nbsp;       result = subprocess.run(

&nbsp;           \[

&nbsp;               WHISPER\_BIN,

&nbsp;               "-m", MODEL\_PATH,

&nbsp;               "-f", wav\_path,

&nbsp;               "--language", "en",

&nbsp;               "--no-timestamps",

&nbsp;               "--beam-size", "1",

&nbsp;               "--best-of", "1"

&nbsp;           ],

&nbsp;           stdout=subprocess.PIPE,

&nbsp;           stderr=subprocess.DEVNULL,

&nbsp;           text=True

&nbsp;       )



&nbsp;       text = result.stdout.strip().lower()

&nbsp;       return jsonify({"text": text})



&nbsp;   finally:

&nbsp;       os.remove(wav\_path)



if \_\_name\_\_ == "\_\_main\_\_":

&nbsp;   app.run(host="0.0.0.0", port=5000)







**5. Start the STT Server**

source ~/jarvis-backend-venv/bin/activate

python3 ~/stt\_server.py



Expected Output : Running on http://0.0.0.0:5000

&nbsp;\* Running on all addresses (0.0.0.0)

&nbsp;\* Running on http://127.0.0.1:5000

&nbsp;\* Running on http://192.168.0.5:5000





**6. Create Remote STT Client on JARVIS**

Install Dependencies :

source ~/jarvis-venv/bin/activate

pip install requests



Create Remote STT Client : 

nano ~/jarvis/stt/remote\_stt.py



import requests



SERVER\_URL = "http://192.168.0.5:5000/stt"



def transcribe\_remote(wav\_path, timeout=10):

&nbsp;   with open(wav\_path, "rb") as f:

&nbsp;       r = requests.post(

&nbsp;           SERVER\_URL,

&nbsp;           files={"audio": f},

&nbsp;           timeout=timeout

&nbsp;       )



&nbsp;   r.raise\_for\_status()

&nbsp;   return r.json().get("text", "")





**7. Modify the main\_loop.py**

ADD  :  from jarvis.stt.remote\_stt import transcribe\_remote



REPLACE : text = transcribe(audio\_path)
WITH : 

try:

&nbsp;   text = transcribe\_remote(audio\_path)

except Exception as e:

&nbsp;   print("⚠️ Server STT failed, falling back:", e)

&nbsp;   text = transcribe(audio\_path)





**8. Mic Ownership Handling**

Since only one process can access the microphone at a time, we need to manage when Porcupine takes hold of the mic, and when it hands it over to STT.



###### **pv\_wake\_listener.py**



import pvporcupine

import numpy as np

import scipy.signal





class PorcupineWakeListener:

&nbsp;   def \_\_init\_\_(self, access\_key, keyword\_path, mic, sensitivity=0.98):

&nbsp;       self.mic = mic



&nbsp;       self.porcupine = pvporcupine.create(

&nbsp;           access\_key=access\_key,

&nbsp;           keyword\_paths=\[keyword\_path],

&nbsp;           sensitivities=\[sensitivity]

&nbsp;       )



&nbsp;       self.device\_rate = mic.rate

&nbsp;       self.target\_rate = self.porcupine.sample\_rate



&nbsp;       self.downsample = int(self.device\_rate / self.target\_rate)

&nbsp;       self.frame\_len = self.porcupine.frame\_length \* self.downsample



&nbsp;   def wait\_for\_wake(self):

&nbsp;       while True:

&nbsp;           pcm = self.mic.read(self.frame\_len)



&nbsp;           pcm16 = scipy.signal.resample\_poly(

&nbsp;               pcm,

&nbsp;               up=1,

&nbsp;               down=self.downsample

&nbsp;           ).astype(np.int16)



&nbsp;           if self.porcupine.process(pcm16.tolist()) >= 0:

&nbsp;               return True



&nbsp;   def close(self):

&nbsp;       self.porcupine.delete()







###### **main\_loop.py**



import os

import soundfile as sf



from jarvis.audio.mic import SharedMic, record\_command\_from\_stream

from jarvis.wakeword.pv\_wake\_listener import PorcupineWakeListener

from jarvis.audio.ack\_player import play\_ack

from jarvis.stt.remote\_stt import transcribe\_remote

from jarvis.stt.whisper\_stt import transcribe

from jarvis.tts.piper\_tts import speak





ACCESS\_KEY = "E2N/iS3toDAjAsdeJf9YfIpZAmVNC6cxrXTpV0K+DOlguNk2spPkuA=="

KEYWORD\_PATH = os.path.expanduser("~/jarvis/wakeword/models/jarvis.ppn")





def main():

&nbsp;   print("🧠 Jarvis online")



&nbsp;   mic = SharedMic(rate=48000)



&nbsp;   wake\_listener = PorcupineWakeListener(

&nbsp;       access\_key=ACCESS\_KEY,

&nbsp;       keyword\_path=KEYWORD\_PATH,

&nbsp;       mic=mic

&nbsp;   )



&nbsp;   try:

&nbsp;       while True:

&nbsp;           print("🎧 Waiting for wake word...")

&nbsp;           wake\_listener.wait\_for\_wake()

&nbsp;           print("🟢 Wake detected")



&nbsp;           play\_ack()



&nbsp;           audio = record\_command\_from\_stream(mic)



&nbsp;           if audio is None:

&nbsp;               print("⚠️ No command audio")

&nbsp;               continue



&nbsp;           wav\_path = "/tmp/jarvis\_cmd.wav"

&nbsp;           sf.write(wav\_path, audio, 48000)



&nbsp;           try:

&nbsp;               text = transcribe\_remote(wav\_path)

&nbsp;           except Exception:

&nbsp;               text = transcribe(wav\_path)



&nbsp;           print(f"📝 Command: {text}")



&nbsp;           if text.strip():

&nbsp;               speak("I heard you.")



&nbsp;   except KeyboardInterrupt:

&nbsp;       print("\\n🛑 Shutdown")



&nbsp;   finally:

&nbsp;       wake\_listener.close()

&nbsp;       mic.close()

&nbsp;       print("✅ Clean exit")





if \_\_name\_\_ == "\_\_main\_\_":

&nbsp;   main()





###### **jarvis/audio/mic.py**



import pyaudio

import numpy as np

import time





class SharedMic:

&nbsp;   def \_\_init\_\_(self, rate=48000, channels=1, device\_index=None):

&nbsp;       self.rate = rate

&nbsp;       self.channels = channels

&nbsp;       self.device\_index = device\_index



&nbsp;       self.pa = pyaudio.PyAudio()

&nbsp;       self.stream = self.pa.open(

&nbsp;           format=pyaudio.paInt16,

&nbsp;           channels=self.channels,

&nbsp;           rate=self.rate,

&nbsp;           input=True,

&nbsp;           input\_device\_index=self.device\_index,

&nbsp;           frames\_per\_buffer=1024

&nbsp;       )



&nbsp;   def read(self, frames):

&nbsp;       data = self.stream.read(frames, exception\_on\_overflow=False)

&nbsp;       return np.frombuffer(data, dtype=np.int16)



&nbsp;   def close(self):

&nbsp;       self.stream.stop\_stream()

&nbsp;       self.stream.close()

&nbsp;       self.pa.terminate()





\# -----------------------------

\# Command Recording (silence based)

\# -----------------------------

def record\_command\_from\_stream(

&nbsp;   mic: SharedMic,

&nbsp;   max\_seconds=3.0,

&nbsp;   silence\_timeout=0.4,

&nbsp;   silence\_threshold=500

):

&nbsp;   frames = \[]

&nbsp;   start = time.time()

&nbsp;   silence\_start = None



&nbsp;   while time.time() - start < max\_seconds:

&nbsp;       chunk = mic.read(1024)

&nbsp;       frames.append(chunk)



&nbsp;       volume = np.abs(chunk).mean()



&nbsp;       if volume < silence\_threshold:

&nbsp;           if silence\_start is None:

&nbsp;               silence\_start = time.time()

&nbsp;           elif time.time() - silence\_start > silence\_timeout:

&nbsp;               break

&nbsp;       else:

&nbsp;           silence\_start = None



&nbsp;   if not frames:

&nbsp;       return None



&nbsp;   audio = np.concatenate(frames)

&nbsp;   return audio







**9. Fixing \[blank\_audio] recordings**

Resample to 16KHz on the Pi (main\_loop.py)



import scipy.signal

import numpy as np



TARGET\_SR = 16000



def write\_wav\_16k(path, audio\_48k):

&nbsp;   audio\_16k = scipy.signal.resample\_poly(

&nbsp;       audio\_48k,

&nbsp;       up=1,

&nbsp;       down=3

&nbsp;   ).astype(np.int16)



&nbsp;   sf.write(path, audio\_16k, TARGET\_SR)











**INSTALL WHISPER.CPP WITH CUDA BUILD**



1. **Clear off past Whisper builds**

cd ~/whisper.cpp

rm -rf build



**2. Build Whisper.cpp with CUDA**

sudo apt update

sudo apt install -y \\

&nbsp; build-essential cmake git \\

&nbsp; libopenblas-dev \\

&nbsp; nvidia-cuda-toolkit



**3. Configure CUDA Build**

cmake -B build \\

&nbsp; -DGGML\_CUDA=ON \\

&nbsp; -DGGML\_BLAS=ON \\

&nbsp; -DGGML\_BLAS\_VENDOR=OpenBLAS \\

&nbsp; -DCMAKE\_BUILD\_TYPE=Release



**4. Compile the Build**

cmake --build build -j$(nproc)







