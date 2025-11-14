# Air Canvas - Hand Gesture Drawing System

A real-time air canvas drawing application using hand gesture recognition powered by MediaPipe and TensorFlow.

## Project Structure


Drawing-System/
├── code/
│   ├── air_canvas.py          # Main drawing application
│   ├── train_model.py         # Model training script
│   ├── color_palette.py       # Color picker utility
│   ├── gesture_model.h5       # Trained gesture recognition model
│   └── class_mapping.json     # Gesture class labels
├── dataset/
│   ├── Bye/                   # Training images for "Bye" gesture
│   ├── Hello/                 # Training images for "Hello" gesture
│   ├── No/                    # Training images for "No" gesture
│   ├── Perfect/               # Training images for "Perfect" gesture
│   ├── Thank You/             # Training images for "Thank You" gesture
│   └── Yes/                   # Training images for "Yes" gesture
└── README.md


## Installation

### Prerequisites
- Python 3.8+
- Webcam
- Windows/Mac/Linux

### Install Dependencies

bash
pip install opencv-python mediapipe tensorflow numpy


Or use the requirements file:
bash
pip install -r requirements.txt


## Usage

### 1. Train the Gesture Recognition Model

bash
python code/train_model.py


This will:
- Load images from dataset/ folder
- Train a CNN model on gesture images (80% train / 20% validation)
- Save gesture_model.h5 and class_mapping.json to code/
- Takes ~10 epochs (varies by hardware)

*Expected output:*

Using dataset at: ...
Model saved to ...


### 2. Run the Air Canvas Application

bash
python code/air_canvas.py


*Controls:*

| Gesture | Action |
|---------|--------|
| *Hello* | Enable drawing |
| *No* | Disable drawing |
| *Perfect* | Increase brush size |
| *Bye* | Decrease brush size |
| *Yes* | Save canvas |
| *Thumb-Index Pinch* | Draw (fallback when model unavailable) |
| *Press "CLEAR" button* | Clear canvas |
| *Press "COLOR" button* | Open color picker |
| *Press "SAVE" button* | Save drawing |
| *Press 'q'* | Exit application |

### 3. Drawing Tips

- *Index finger extended*: Marks drawing position
- *Index-only pose* (when ML model unavailable): Enables drawing
- *Thumb-to-index pinch*: Fallback drawing trigger if gesture model fails
- Point your index finger at the screen to draw
- Use gestures to toggle drawing on/off and adjust brush size

## Training Your Own Model

1. *Collect gesture images* and organize them in dataset/ folders:
   
   dataset/
   ├── Bye/
   ├── Hello/
   ├── No/
   ├── Perfect/
   ├── Thank You/
   └── Yes/
   

2. *Run training*:
   bash
   python code/train_model.py
   

3. *Use the trained model* in Air Canvas automatically

## Model Architecture

- *Input*: 128×128 RGB images
- *Layers*:
  - Conv2D(32) + MaxPool → Conv2D(64) + MaxPool → Conv2D(128) + MaxPool
  - Flatten → Dense(128, relu, dropout=0.4) → Dense(num_classes, softmax)
- *Training*: Adam optimizer, learning rate 1e-4, 10 epochs
- *Data augmentation*: Rotation, zoom, shifts, horizontal flip

## Troubleshooting

### "No gesture model found"
- Train the model first: python code/train_model.py
- Ensure dataset folder exists with gesture images

### Application won't start
- Check camera is connected and not in use
- Verify all dependencies installed: pip install opencv-python mediapipe tensorflow

### Drawing not working
- Ensure your index finger is clearly visible to the camera
- Try the fallback: pinch thumb and index finger to draw
- Increase ambient lighting for better hand detection

### Model training fails
- Verify dataset folder structure with gesture subdirectories
- Check image format (JPEG, PNG supported)
- Ensure minimum images per class (~50-100 recommended)

## Performance

- *FPS*: ~25-30 (depends on hardware)
- *Latency*: ~50-100ms (hand detection + gesture prediction)
- *Memory*: ~500MB-1GB during training, ~200MB during inference

## File Descriptions

| File | Purpose |
|------|---------|
| air_canvas.py | Main drawing app with real-time hand detection and gesture recognition |
| train_model.py | CNN training pipeline with data augmentation |
| color_palette.py | Tkinter color picker widget |
| gesture_model.h5 | Trained Keras model for gesture classification |
| class_mapping.json | Gesture class index-to-name mapping |

## License

Open source for educational purposes.