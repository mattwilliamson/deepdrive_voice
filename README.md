# deepdrive_voice

TODO: PR for 
```python
    def int2float(self, sound):
        abs_max = np.abs(sound).max()
        sound = sound.astype('float32')
        if abs_max > 0:
            sound *= 1/32768
        sound = sound.squeeze()  # depends on the use case
        return sound
```

TODO: no module named "expiringdict"


```
make docker_run
```

on machine with speakers and microphone:
```sh
ros2 run audio_common audio_player_node --ros-args -r audio:=audio/out
ros2 run audio_common audio_capturer_node  --ros-args -r audio:=audio/in
```

on machine with gpu
```sh
ros2 launch deepdrive_voice_bringup chatbot.launch.py
ros2 launch chatbot_bringup chatbot.launch.py

#  --ros-args -p launch_audio_capturer:=False -p launch_audio_player:=False
# --ros-args -r __node:=llama_node -r __ns:=/llama 

ros2 launch foxglove_bridge foxglove_bridge_launch.xml

```

```sh
ros2 launch deepdrive_voice_bringup marcoroni.launch.py
ros2 launch deepdrive_voice_bringup llama3.launch.py
ros2 launch deepdrive_voice_bringup hermes.launch.py



ros2 run llama_ros llama_demo_node --ros-args -p prompt:="why is america called america?"



# model_repo="TheBloke/Marcoroni-7B-v3-GGUF",
# model_filename="marcoroni-7b-v3.Q4_K_M.gguf",

# ros2 run tts_ros tts_node --ros-args -p chunk:=4096 -p frame_id:="base_link" -p model:="your-model" -p speaker_wav:="/path/to/wav/file" device:="cuda:0"

ros2 action send_goal /say audio_common_msgs/action/TTS "{'text': 'The name America is derived from the Native American word Amerika, which means land of the free or land of to the continent by the indigenous peoples who inhabited it before the arrival of Europeans. The name was later adopted by Europeans who arrived in the region and established the United States of America. The name America is used to refer to the country and its people, as well as to the continent of North America.'}"
```

### Uncomment in `src/llama_ros/llama_ros/CMakeLists.txt`
```
option(LLAMA_CUDA "llama: use CUDA" ON)
add_compile_definitions(GGML_USE_CUDA)
```

```
in launch file:
n_gpu_layers = 99
```

### Uncomment in `src/whisper_ros/whisper_ros/CMakeLists.txt`
```
option(WHISPER_CUBLAS "whisper: support for cuBLAS" ON)
```


---

TTS Models

```sh
root@hugger:~/ros2_ws# tts --list_models
/usr/local/lib/python3.10/dist-packages/matplotlib/projections/__init__.py:63: UserWarning: Unable to import Axes3D. This may be due to multiple versions of Matplotlib being installed (e.g. as a system package and as a pip package). As a result, the 3D projection is not available.
  warnings.warn("Unable to import Axes3D. This may be due to multiple versions of "

 Name format: type/language/dataset/model
 1: tts_models/multilingual/multi-dataset/xtts_v2
 2: tts_models/multilingual/multi-dataset/xtts_v1.1
 3: tts_models/multilingual/multi-dataset/your_tts
 4: tts_models/multilingual/multi-dataset/bark
 5: tts_models/bg/cv/vits
 6: tts_models/cs/cv/vits
 7: tts_models/da/cv/vits
 8: tts_models/et/cv/vits
 9: tts_models/ga/cv/vits
 10: tts_models/en/ek1/tacotron2
 11: tts_models/en/ljspeech/tacotron2-DDC
 12: tts_models/en/ljspeech/tacotron2-DDC_ph
 13: tts_models/en/ljspeech/glow-tts
 14: tts_models/en/ljspeech/speedy-speech
 15: tts_models/en/ljspeech/tacotron2-DCA
 16: tts_models/en/ljspeech/vits
 17: tts_models/en/ljspeech/vits--neon
 18: tts_models/en/ljspeech/fast_pitch
 19: tts_models/en/ljspeech/overflow
 20: tts_models/en/ljspeech/neural_hmm
 21: tts_models/en/vctk/vits
 22: tts_models/en/vctk/fast_pitch
 23: tts_models/en/sam/tacotron-DDC
 24: tts_models/en/blizzard2013/capacitron-t2-c50
 25: tts_models/en/blizzard2013/capacitron-t2-c150_v2
 26: tts_models/en/multi-dataset/tortoise-v2
 27: tts_models/en/jenny/jenny
 28: tts_models/es/mai/tacotron2-DDC
 29: tts_models/es/css10/vits
 30: tts_models/fr/mai/tacotron2-DDC
 31: tts_models/fr/css10/vits
 32: tts_models/uk/mai/glow-tts
 33: tts_models/uk/mai/vits
 34: tts_models/zh-CN/baker/tacotron2-DDC-GST
 35: tts_models/nl/mai/tacotron2-DDC
 36: tts_models/nl/css10/vits
 37: tts_models/de/thorsten/tacotron2-DCA
 38: tts_models/de/thorsten/vits
 39: tts_models/de/thorsten/tacotron2-DDC
 40: tts_models/de/css10/vits-neon
 41: tts_models/ja/kokoro/tacotron2-DDC
 42: tts_models/tr/common-voice/glow-tts
 43: tts_models/it/mai_female/glow-tts
 44: tts_models/it/mai_female/vits
 45: tts_models/it/mai_male/glow-tts
 46: tts_models/it/mai_male/vits
 47: tts_models/ewe/openbible/vits
 48: tts_models/hau/openbible/vits
 49: tts_models/lin/openbible/vits
 50: tts_models/tw_akuapem/openbible/vits
 51: tts_models/tw_asante/openbible/vits
 52: tts_models/yor/openbible/vits
 53: tts_models/hu/css10/vits
 54: tts_models/el/cv/vits
 55: tts_models/fi/css10/vits
 56: tts_models/hr/cv/vits
 57: tts_models/lt/cv/vits
 58: tts_models/lv/cv/vits
 59: tts_models/mt/cv/vits
 60: tts_models/pl/mai_female/vits
 61: tts_models/pt/cv/vits
 62: tts_models/ro/cv/vits
 63: tts_models/sk/cv/vits
 64: tts_models/sl/cv/vits
 65: tts_models/sv/cv/vits
 66: tts_models/ca/custom/vits
 67: tts_models/fa/custom/glow-tts
 68: tts_models/bn/custom/vits-male
 69: tts_models/bn/custom/vits-female
 70: tts_models/be/common-voice/glow-tts

 Name format: type/language/dataset/model
 1: vocoder_models/universal/libri-tts/wavegrad
 2: vocoder_models/universal/libri-tts/fullband-melgan
 3: vocoder_models/en/ek1/wavegrad
 4: vocoder_models/en/ljspeech/multiband-melgan
 5: vocoder_models/en/ljspeech/hifigan_v2
 6: vocoder_models/en/ljspeech/univnet
 7: vocoder_models/en/blizzard2013/hifigan_v2
 8: vocoder_models/en/vctk/hifigan_v2
 9: vocoder_models/en/sam/hifigan_v2
 10: vocoder_models/nl/mai/parallel-wavegan
 11: vocoder_models/de/thorsten/wavegrad
 12: vocoder_models/de/thorsten/fullband-melgan
 13: vocoder_models/de/thorsten/hifigan_v1
 14: vocoder_models/ja/kokoro/hifigan_v1
 15: vocoder_models/uk/mai/multiband-melgan
 16: vocoder_models/tr/common-voice/hifigan
 17: vocoder_models/be/common-voice/hifigan

 Name format: type/language/dataset/model
 1: voice_conversion_models/multilingual/vctk/freevc24
 ```