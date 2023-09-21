## How does it work?
- Music Source Separation is performed with the [Demucs](https://github.com/facebookresearch/demucs) neural network
- Music Structure Segmentation/Labeling is performed with the [sf_segmenter](https://github.com/wayne391/sf_segmenter) neural network
- Music Pitch Tracking and Key Detection are performed with [Crepe](https://github.com/marl/crepe) neural network
- Music to MIDI transcription is performed with [Basic Pitch](https://github.com/spotify/basic-pitch) neural network
- Music Quantization and Alignment are performed with [pyrubberband](https://github.com/bmcfee/pyrubberband)
- Music Info retrieval and processing is performed with [librosa](https://github.com/librosa/librosa)

## Requirements

You need to have the following software installed on your system:

- ``ffmpeg``

If you run into an issue with basic-pitch while trying to run intellitunez, run this command after your installation:
```bash
pip install git+https://github.com/spotify/basic-pitch.git
```

## GPU support

Most of the libraries intellitunez uses come with native GPU support through cuda. Please follow the steps on https://www.tensorflow.org/install/pip to setup tensorflow for use with cuda. If you have followed these steps, tensorflow and torch will both automatically pick up the GPU and use it. This only applied to native setups, for dockerized deployments (see next section), gpu support is forthcoming

## Run intellitunez

### 1. Add songs to the intellitunez Library

##### Add YouTube video to library (auto-download)
```bash
python intellitunez.py -a n6DAqMFe97E
```
##### Add audio file (wav or mp3)
```bash
python intellitunez.py -a /path/to/audiolib/song.wav
```
##### Add multiple files at once
```bash
python intellitunez.py -a n6DAqMFe97E,eaPzCHEQExs,RijB8wnJCN0
python intellitunez.py -a /path/to/audiolib/song1.wav,/path/to/audiolib/song2.wav
python intellitunez.py -a /path/to/audiolib/
```
Songs are automatically analyzed once which takes some time. Once in the database, they can be access rapidly. The database is stored in the folder "/library/database.p". To reset everything, simply delete it.

### 2. Quantize songs in the intellitunez Library
##### Quantize a specific songs in the library to tempo 120 BPM (-q = database audio file ID, -t = tempo in BPM)
```bash
python intellitunez.py -q n6DAqMFe97E -t 120
```
##### Quantize all songs in the library to tempo 120 BPM
```bash
python intellitunez.py -q all -t 120
```
##### Quantize a specific songs in the library to the tempo of the song (-k)
```bash
python intellitunez.py -q n6DAqMFe97E -k
```
Songs are automatically quantized to the same tempo and beat-grid and saved to the folder “/processed”.

### 3. Search for similar songs in the intellitunez Library
##### Search for 10 similar songs based on a specific songs in the library (-s = database audio file ID, -sa = results amount)
```bash
python intellitunez.py -s n6DAqMFe97E -sa 10
```
##### Search for similar songs based on a specific songs in the library and quantize all of them to tempo 120 BPM
```bash
python intellitunez.py -s n6DAqMFe97E -sa 10 -q all -t 120
```
##### Include BPM as search criteria  (-st)
```bash
python intellitunez.py -s n6DAqMFe97E -sa 10 -q all -t 120 -st -k
```
Similar songs are automatically found and optionally quantized and saved to the folder "/processed". This makes it easy to create for example an hour long mix of songs that perfectly match one after the other. 

### 4. Convert Audio to MIDI
##### Convert all processed audio files and stems to MIDI (-m)
```bash
python intellitunez.py -a n6DAqMFe97E -q all -t 120 -m
```
Generated Midi Files are currently always 120BPM and need to be time adjusted in your DAW. This will be resolved [soon](https://github.com/spotify/basic-pitch/issues/40). The current Audio2Midi model gives mixed results with drums/percussion. This will be resolved with additional audio2midi model options in the future.


## Audio Features

### Extracted Stems
The Demucs Neural Net has settings that can be adjusted in the python file
```bash
- bass
- drum
- guitare
- other
- piano
- vocals
```
### Extracted Features
The audio feature extractors have settings that can be adjusted in the python file
```bash
- tempo
- duration
- timbre
- timbre_frames
- pitch
- pitch_frames
- intensity
- intensity_frames
- volume
- avg_volume
- loudness
- beats
- segments_boundaries
- segments_labels
- frequency_frames
- frequency
- key
```


