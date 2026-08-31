# Hand Gesture Recognition

A real-time hand gesture recognition system built with **OpenCV**, **cvzone**, and a **Keras** (Teachable Machine) classification model. The webcam feed is used to detect a hand, crop and normalize it into a fixed-size white-background image, and classify it into one of several predefined gestures.

## Features

- Real-time hand detection using `cvzone`'s `HandDetector`
- Automatic cropping and aspect-ratio-aware resizing of the hand region onto a 300x300 white canvas
- Gesture classification using a Keras model trained via [Teachable Machine](https://teachablemachine.withgoogle.com/)
- Custom dataset collection tool to capture and label your own gesture images
- Live on-screen prediction label and bounding box overlay

## Recognized Gestures

```
Hello, Thumbs Up, Yes, Thank You, No, Thumbs Down, Peace, Let's Go
```

## Dataset

This project uses a **custom, self-collected dataset** captured with `datacollection.py`. For each single-hand gesture/sign, **120 images** were collected (cropped, aspect-ratio-normalized, and saved on a 300x300 white background) before being used to train the classification model via Teachable Machine.

## Project Structure

```
.
├── main.py                # Runs real-time detection + classification
├── datacollection.py      # Captures hand images for building a training dataset
├── shutdown.py            # Utility script to restart the system (Windows)
└── converted_keras/
    ├── keras_model.h5     # Trained Keras model (exported from Teachable Machine)
    └── labels.txt          # Corresponding class labels
```

## Requirements

- Python 3.7+
- A working webcam

Install dependencies:

```bash
pip install opencv-python cvzone numpy tensorflow
```

> `cvzone`'s `ClassificationModule` depends on `tensorflow`/`keras` to load the `.h5` model.

## Usage

### 1. Collect training data (optional)

If you want to train your own model on new gestures, use `datacollection.py`:

```bash
python datacollection.py
```

- Point your hand at the webcam.
- Press **`s`** to save a cropped/normalized snapshot to the configured output folder.
- Update the `folder` variable in the script to match the gesture you're capturing, and repeat for each gesture class.
- Once you have enough samples per class, train a model using [Teachable Machine](https://teachablemachine.withgoogle.com/) (or your own pipeline) and export it as a Keras model (`keras_model.h5` + `labels.txt`).

### 2. Run gesture recognition

```bash
python main.py
```

- Update the model/label paths in `main.py` to point to your `converted_keras/keras_model.h5` and `converted_keras/labels.txt`.
- The app opens your webcam, detects your hand, and overlays the predicted gesture label in real time.
- Press **`Esc`** or close the window to quit.

## Notes

- Paths in the scripts are currently hardcoded to a local Windows directory (`D:\project\...`). Update these to relative paths before pushing/running on another machine.
- `shutdown.py` is a standalone utility that restarts a Windows machine (`shutdown /r /t 1`) and is not wired into the gesture pipeline by default — hook it up if you'd like a gesture (e.g. a specific label) to trigger a system action.
- contributed 

## License

This project is open source and available under the [MIT License](LICENSE).
