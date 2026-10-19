# NEA Rhythm Game

A rhythm game written in Python for my A-level Computer Science NEA. It analyses a WAV file to estimate beat times, then drops notes in time with the song. You hit the notes with the keyboard or with your hand through a webcam.

## What it does

- **Beat detection** (`beat_finding_algorithm.py`): reads a WAV file, smooths the signal with a moving average and FFT low-pass filtering, then finds turning points in the waveform. These are merged with a grid built from the song's BPM, and the resulting note times are written to `turning_points.txt`.
- **Game** (`main_game.py`): a Pygame window where notes fall down configurable lanes in time with the song. You score 2 points for a note fully inside the red hit zone and 1 point for a partial overlap.
- **Input modes**:
  - `no_hands`: keyboard only (`F/D/S/A...` for the left lanes, `H/J/K/L...` for the right lanes)
  - `freehands`: webcam hand tracking with MediaPipe; your index fingertip hits notes
  - `fixed_hands`: hand tracking with the fingertip locked to a fixed height near the hit zone
- **Menu and settings** (`main_menu_ui.py`): a Tkinter menu with a play button and a settings window for FPS, note speed, number of lanes and input mode. Settings are saved to `settings_vars.txt`.
- `beat_finding_graphs_txts/` holds logs from tuning the beat-detection parameters on several songs.

## Tech

- Python 3.8
- Pygame, NumPy, SciPy, simpleaudio, OpenCV (`cv2`), MediaPipe (legacy `mp.solutions.hands` API), Tkinter, Matplotlib

## Running it

There is no requirements file. Install the libraries listed above, then run these commands from the repo root, because the code uses relative paths:

```
python beat_finding_algorithm.py   # generates turning_points.txt for the song set at the top of the file
python main_menu_ui.py             # opens the menu; Play starts the game
```

- Songs are not included in the repo (copyright). Put your own WAV file in `wav_files/`, for example `wav_files/my_song.wav`.
- The song is hard-coded: `wav_file_name` in `main_game.py`  and `wav_file_name`/`song_bpm` in `beat_finding_algorithm.py`. Set both to the same song.
- `testing_algorithm.py` runs beat detection and then the game for a single song in one step.
- A webcam is opened at startup, even in `no_hands` mode.

## Notes

This was school coursework (2023). Some files are prototypes or test scripts (`testing_ui.py`, `settings_ui.py`, `beat_playing_algorithm.py`, `main_game_no_comments.py`).
