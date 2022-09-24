
# whisper_streaming (beta version)

[![CI](https://github.com/shirayu/whisper_streaming/actions/workflows/ci.yml/badge.svg)](https://github.com/shirayu/whisper_streaming/actions/workflows/ci.yml)
[![CodeQL](https://github.com/shirayu/whisper_streaming/actions/workflows/codeql-analysis.yml/badge.svg)](https://github.com/shirayu/whisper_streaming/actions/workflows/codeql-analysis.yml)
[![Typos](https://github.com/shirayu/whisper_streaming/actions/workflows/typos.yml/badge.svg)](https://github.com/shirayu/whisper_streaming/actions/workflows/typos.yml)

Streaming transcriber with [whisper](https://github.com/openai/whisper).
Transcribing in real time, enough machine power is needed.

## Setup

```bash
git clone https://github.com/shirayu/whisper_streaming.git
cd whisper_streaming
poetry install --only main

# If you use GPU, install proper torch and torchaudio with "poetry run pip install -U"
# Example : torch for CUDA 11.6
poetry run pip install -U torch torchaudio --extra-index-url https://download.pytorch.org/whl/cu116
```

## Example of microphone

```bash
# Run in English
poetry run whisper_streaming --language en --model base
```

- ``--help`` shows full options
- ``--language`` sets the language to transcribe. The list of languages are shown with ``poetry run whisper_streaming -h``
- ``-t`` sets temperatures to decode. You can set several like (``-t 0.0 -t 0.1 -t 0.5``), but too many temperatures exhaust decoding time
- ``--debug`` outputs logs for debug

### Parse interval

If you want quick response, set small ``-n`` and add ``--allow-padding``.
However, this may be at the sacrifice of accuracy.

```bash
poetry run whisper_streaming --language en --model base -n 20 --allow-padding
```

## Example of web socket

⚠  **No security mechanism. Please make secure with your responsibility.**

### Host

Run with ``--host`` and ```--port``. You can set``-n`` and other options as use with microphone.

```bash
poetry run whisper_streaming --language en --model base --host 0.0.0.0 --port 8000
```

### Client

```bash
poetry run python -m whisper_streaming.websocket_client --host ADDRESS_OF_HOST --port 8000 
```

## Tips

## PortAudio Error

If you get ``OSError: PortAudio library not found``: Install ``portaudio``

```bash
# Ubuntu
sudo apt-get install portaudio19-dev
```

## License

- [MIT License](LICENSE)
- Some codes are ported from the original whisper. Its license is also [MIT License](LICENSE.whisper)
