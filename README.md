# absadiki's pywhispercpp

pywhispercpp is a fork from [Absadiki's](https://github.com/jt-notes/absadiki_pywhispercpp) Python bindings for that model. [whisper.cpp](https://github.com/ggerganov/whisper.cpp itself is a high-performance inference of automatic speech recognition (ASR) model. It is a plain C/C++ implementation that runs on CPU without dependencies. 

Supported platforms: Mac, Windows, Linux, Android

# Installation

### Install [ffmpeg](https://ffmpeg.org/)

 ```bash
 # on Ubuntu or Debian
 sudo apt update && sudo apt install ffmpeg

 # on Arch Linux
sudo pacman -S ffmpeg

 # on MacOS using Homebrew (https://brew.sh/)
 brew install ffmpeg

 # on Windows using Chocolatey (https://chocolatey.org/)
 choco install ffmpeg

 # on Windows using Scoop (https://scoop.sh/)
scoop install ffmpeg
```

## Pip installs 

### 2a. Install `pywhispercpp`

```shell
pip install pywhispercpp
```

### 3. Optional: examples
If you want to use the examples, you will need to install extra dependencies

```shell
pip install pywhispercpp[examples]
```



## Alternative source install

### You can install the latest dev version from GitHub:

```shell
git clone --recursive https://github.com/absadiki/pywhispercpp
cd pywhispercpp
pip install .
```


# Quick start

```python
from pywhispercpp.model import Model

model = Model('base.en', n_threads=6)
segments = model.transcribe('file.mp3')
for segment in segments:
    print(segment.text)
```

You can also assign a custom `new_segment_callback`

```python
from pywhispercpp.model import Model

model = Model('base.en', print_realtime=False, print_progress=False)
segments = model.transcribe('file.mp3', new_segment_callback=print)
```


* The `ggml` model will be downloaded automatically.
* You can pass any `whisper.cpp` [parameter](https://absadiki.github.io/pywhispercpp/#pywhispercpp.constants.PARAMS_SCHEMA) as a keyword argument to the `Model` class or to the `transcribe` function.
* The `transcribe` function accepts any media file (audio/video), in any format.
* Check the [Model](https://absadiki.github.io/pywhispercpp/#pywhispercpp.model.Model) class documentation for more details.

# Examples

The [examples folder](https://github.com/absadiki/pywhispercpp/tree/main/pywhispercpp/examples) contains several examples inspired from the original [whisper.cpp/examples](https://github.com/ggerganov/whisper.cpp/tree/master/examples).

## Main
Just a straightforward example with a simple Command Line Interface. 

Check the source code [here](https://github.com/absadiki/pywhispercpp/blob/main/pywhispercpp/examples/main.py), or use the CLI as follows:

```shell
pwcpp file.wav -m base --output-srt --print_realtime true
```
Run ```pwcpp --help``` to get the help message

```shell
usage: pwcpp [-h] [-m MODEL] [--version] [--processors PROCESSORS] [-otxt] [-ovtt] [-osrt] [-ocsv] [--strategy STRATEGY]
             [--n_threads N_THREADS] [--n_max_text_ctx N_MAX_TEXT_CTX] [--offset_ms OFFSET_MS] [--duration_ms DURATION_MS]
             [--translate TRANSLATE] [--no_context NO_CONTEXT] [--single_segment SINGLE_SEGMENT] [--print_special PRINT_SPECIAL]
             [--print_progress PRINT_PROGRESS] [--print_realtime PRINT_REALTIME] [--print_timestamps PRINT_TIMESTAMPS]
             [--token_timestamps TOKEN_TIMESTAMPS] [--thold_pt THOLD_PT] [--thold_ptsum THOLD_PTSUM] [--max_len MAX_LEN]
             [--split_on_word SPLIT_ON_WORD] [--max_tokens MAX_TOKENS] [--audio_ctx AUDIO_CTX]
             [--prompt_tokens PROMPT_TOKENS] [--prompt_n_tokens PROMPT_N_TOKENS] [--language LANGUAGE] [--suppress_blank SUPPRESS_BLANK]
             [--suppress_non_speech_tokens SUPPRESS_NON_SPEECH_TOKENS] [--temperature TEMPERATURE] [--max_initial_ts MAX_INITIAL_TS]
             [--length_penalty LENGTH_PENALTY] [--temperature_inc TEMPERATURE_INC] [--entropy_thold ENTROPY_THOLD]
             [--logprob_thold LOGPROB_THOLD] [--no_speech_thold NO_SPEECH_THOLD] [--greedy GREEDY] [--beam_search BEAM_SEARCH]
             media_file [media_file ...]

positional arguments:
  media_file            The path of the media file or a list of filesseparated by space

options:
  -h, --help            show this help message and exit
  -m MODEL, --model MODEL
                        Path to the `ggml` model, or just the model name
  --version             show program's version number and exit
  --processors PROCESSORS
                        number of processors to use during computation
  -otxt, --output-txt   output result in a text file
  -ovtt, --output-vtt   output result in a vtt file
  -osrt, --output-srt   output result in a srt file
  -ocsv, --output-csv   output result in a CSV file
  --strategy STRATEGY   Available sampling strategiesGreefyDecoder -> 0BeamSearchDecoder -> 1
  --n_threads N_THREADS
                        Number of threads to allocate for the inferencedefault to min(4, available hardware_concurrency)
  --n_max_text_ctx N_MAX_TEXT_CTX
                        max tokens to use from past text as prompt for the decoder
  --offset_ms OFFSET_MS
                        start offset in ms
  --duration_ms DURATION_MS
                        audio duration to process in ms
  --translate TRANSLATE
                        whether to translate the audio to English
  --no_context NO_CONTEXT
                        do not use past transcription (if any) as initial prompt for the decoder
  --single_segment SINGLE_SEGMENT
                        force single segment output (useful for streaming)
  --print_special PRINT_SPECIAL
                        print special tokens (e.g. <SOT>, <EOT>, <BEG>, etc.)
  --print_progress PRINT_PROGRESS
                        print progress information
  --print_realtime PRINT_REALTIME
                        print results from within whisper.cpp (avoid it, use callback instead)
  --print_timestamps PRINT_TIMESTAMPS
                        print timestamps for each text segment when printing realtime
  --token_timestamps TOKEN_TIMESTAMPS
                        enable token-level timestamps
  --thold_pt THOLD_PT   timestamp token probability threshold (~0.01)
  --thold_ptsum THOLD_PTSUM
                        timestamp token sum probability threshold (~0.01)
  --max_len MAX_LEN     max segment length in characters
  --split_on_word SPLIT_ON_WORD
                        split on word rather than on token (when used with max_len)
  --max_tokens MAX_TOKENS
                        max tokens per segment (0 = no limit)
  --audio_ctx AUDIO_CTX
                        overwrite the audio context size (0 = use default)
  --prompt_tokens PROMPT_TOKENS
                        tokens to provide to the whisper decoder as initial prompt
  --prompt_n_tokens PROMPT_N_TOKENS
                        tokens to provide to the whisper decoder as initial prompt
  --language LANGUAGE   for auto-detection, set to None, "" or "auto"
  --suppress_blank SUPPRESS_BLANK
                        common decoding parameters
  --suppress_non_speech_tokens SUPPRESS_NON_SPEECH_TOKENS
                        common decoding parameters
  --temperature TEMPERATURE
                        initial decoding temperature
  --max_initial_ts MAX_INITIAL_TS
                        max_initial_ts
  --length_penalty LENGTH_PENALTY
                        length_penalty
  --temperature_inc TEMPERATURE_INC
                        temperature_inc
  --entropy_thold ENTROPY_THOLD
                        similar to OpenAI's "compression_ratio_threshold"
  --logprob_thold LOGPROB_THOLD
                        logprob_thold
  --no_speech_thold NO_SPEECH_THOLD
                        no_speech_thold
  --greedy GREEDY       greedy
  --beam_search BEAM_SEARCH
                        beam_search

```

## Recording 
Another simple [example](https://github.com/absadiki/pywhispercpp/blob/main/pywhispercpp/examples/recording.py) to transcribe your own recordings.

You can use it from Python as follows:

```python
from pywhispercpp.examples.recording import Recording

myrec = Recording(5)
myrec.start()
```

Or from the command line:
    
```shell
$ pwcpp-recording --help

usage: pwcpp-recording [-h] [-m MODEL] duration

positional arguments:
  duration              duration in seconds

options:
  -h, --help            show this help message and exit
  -m MODEL, --model MODEL
                        Whisper.cpp model, default to tiny.en
```



# Advanced usage
* First check the [API documentation](https://absadiki.github.io/pywhispercpp/) for more advanced usage.

* If you are a more experienced user, you can access the [C-Style API](https://github.com/ggerganov/whisper.cpp/blob/master/whisper.h) directly, almost all functions from `whisper.h`
are exposed with the binding module `_pywhispercpp`.

```python
import _pywhispercpp as pwcpp

ctx = pwcpp.whisper_init_from_file('path/to/ggml/model')
```

# Issues
Please contribute to [Absadiki's](https://github.com/absadiki/pywhispercpp/issues) as this is simply an exploration of that code.

# License

This project is licensed under the same license as [Absadiki's](https://github.com/absadiki/pywhispercpp) Python bindings and as [whisper.cpp](https://github.com/ggerganov/whisper.cpp/blob/master/LICENSE) (MIT  [License](./LICENSE)).

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Wheels](https://github.com/absadiki/pywhispercpp/actions/workflows/wheels.yml/badge.svg?branch=main&event=push)](https://github.com/absadiki/pywhispercpp/actions/workflows/wheels.yml)
[![PyPi version](https://badgen.net/pypi/v/pywhispercpp)](https://pypi.org/project/pywhispercpp/)
[![Downloads](https://static.pepy.tech/badge/pywhispercpp)](https://pepy.tech/project/pywhispercpp)
