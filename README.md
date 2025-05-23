This project demonstrates the implementation of real-time facial emotion recognition using the deepface
 library and OpenCV. The objective is to capture live video from a 
webcam, identify faces within the video stream, and predict the 
corresponding emotions for each detected face. The emotions predicted 
are displayed in real-time on the video frames.


To streamline this process, we've utilized the deepface
 library, a deep learning-based facial analysis tool that employs 
pre-trained models for accurate emotion detection. TensorFlow is the 
underlying framework for the deep learning operations. Additionally, we 
leverage OpenCV, an open-source computer vision library, to facilitate 
image and video processing.


Instructions

Initial Setup:






Navigate to the project directory: Run cd Facial-Emotion-Recognition-using-OpenCV-and-Deepface.




Install required dependencies:


Option 1: Use pip install -r requirements.txt.
Option 2: Install dependencies individually:

pip install deepface
pip install opencv-python






Obtain the Haar cascade XML file for face detection:


Download the haarcascade_frontalface_default.xml file from the OpenCV GitHub repository.




Execute the code:


Run the Python script.
The webcam will activate, initiating real-time facial emotion detection.
Emotion labels will be superimposed onto the frames containing recognized faces.




Approach



Import Essential Libraries: Import cv2 for video capture and image processing, as well as deepface for the emotion detection model.




Load Haar Cascade Classifier: Utilize cv2.CascadeClassifier() to load the XML file for face detection.




Video Capture Initialization: Employ cv2.VideoCapture() to initiate video capture from the default webcam.




Frame Processing Loop: Enter a continuous loop to process each video frame.




Grayscale Conversion: Transform each frame into grayscale using cv2.cvtColor().




Face Detection: Detect faces within the grayscale frame using face_cascade.detectMultiScale().




Face Region Extraction: For each detected face, extract the Region of Interest (ROI) containing the face.




Preprocessing: Prepare the face image for emotion detection by employing the built-in preprocessing function from the deepface library.




Emotion Prediction: Utilize the pre-trained emotion detection model provided by the deepface library to predict emotions.




Emotion Labeling: Map the predicted emotion index to the corresponding emotion label.




Visual Annotation: Draw rectangles around the detected faces and label them with the predicted emotions via cv2.rectangle() and cv2.putText().




Display Output: Present the resulting frame with the labeled emotion using cv2.imshow().




Loop Termination: If the 'q' key is pressed, exit the loop.




Cleanup: Release video capture resources and close all windows with cap.release() and cv2.destroyAllWindows().




If you find this project useful, consider giving it a ⭐ on the repository. The creator, Ajith Kumar M, invested time and effort into comprehending and implementing this efficient real-time emotion monitoring solution.


Sure, let's break down the code step by step:


Import required libraries:


import cv2
from deepface import DeepFace

    
cv2 is the OpenCV library used for computer vision and image processing.
DeepFace is a class from the deepface library used for building and using facial analysis models.


Load the pre-trained emotion detection model:


model = DeepFace.build_model("Emotion")

    
This line creates an instance of the pre-trained emotion detection model provided by the deepface library.


Define emotion labels:


emotion_labels = ['angry', 'disgust', 'fear', 'happy', 'sad', 'surprise', 'neutral']

    
A list of emotion labels corresponding to the detected emotions.


Load face cascade classifier:


face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')

    
Loads the Haar cascade classifier for face detection provided by OpenCV.


Start capturing video:


cap = cv2.VideoCapture(0)

    
Initializes video capture from the default webcam (camera index 0).


Main Loop:


while True:
    ret, frame = cap.read()
    gray_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    faces = face_cascade.detectMultiScale(gray_frame, scaleFactor=1.1, minNeighbors=5, minSize=(30, 30))

    
The loop continuously captures frames from the webcam (cap.read()), converts them to grayscale (cv2.cvtColor()), and detects faces within the grayscale frame using the Haar cascade classifier (face_cascade.detectMultiScale()).


Face Processing Loop:


for (x, y, w, h) in faces:
    face_roi = gray_frame[y:y + h, x:x + w]
    resized_face = cv2.resize(face_roi, (48, 48), interpolation=cv2.INTER_AREA)
    normalized_face = resized_face / 255.0
    reshaped_face = normalized_face.reshape(1, 48, 48, 1)
    preds = model.predict(reshaped_face)[0]
    emotion_idx = preds.argmax()
    emotion = emotion_labels[emotion_idx]
    cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 0, 255), 2)
    cv2.putText(frame, emotion, (x, y - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.9, (0, 0, 255), 2)

    
For each detected face, this block of code processes the face:

Extracts the Region of Interest (ROI) of the face.
Resizes the face to match the input size of the emotion detection model.
Normalizes the resized face image.
Reshapes the image for model input.
Makes emotion predictions using the pre-trained model.
Draws a rectangle around the face and labels it with the predicted emotion using OpenCV functions.




Display and Exit:


cv2.imshow('Real-time Emotion Detection', frame)
if cv2.waitKey(1) & 0xFF == ord('q'):
    break

    
Displays the frame with labeled emotions using cv2.imshow(). The loop continues until the 'q' key is pressed, at which point the loop is exited.


Cleanup:


cap.release()
cv2.destroyAllWindows()

    
Releases the video capture and closes all windows.


In summary, this code captures real-time video from the 
webcam, detects faces, predicts emotions using a pre-trained model, and 
displays the processed frames with labeled emotions. The loop continues 
until the 'q' key is pressed, at which point the program is terminated.


Complete code here:


Real-time Facial Emotion Recognition using OpenCV and Deepface

import cv2
from deepface import DeepFace


Load the pre-trained emotion detection model

model = DeepFace.build_model("Emotion")


Define emotion labels

emotion_labels = ['angry', 'disgust', 'fear', 'happy', 'sad', 'surprise', 'neutral']


Load face cascade classifier

face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')


Start capturing video

cap = cv2.VideoCapture(0)


while True:
# Capture frame-by-frame
ret, frame = cap.read()


# Convert frame to grayscale
gray_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

# Detect faces in the frame
faces = face_cascade.detectMultiScale(gray_frame, scaleFactor=1.1, minNeighbors=5, minSize=(30, 30))

for (x, y, w, h) in faces:
    # Extract the face ROI (Region of Interest)
    face_roi = gray_frame[y:y + h, x:x + w]

    # Resize the face ROI to match the input shape of the model
    resized_face = cv2.resize(face_roi, (48, 48), interpolation=cv2.INTER_AREA)

    # Normalize the resized face image
    normalized_face = resized_face / 255.0

    # Reshape the image to match the input shape of the model
    reshaped_face = normalized_face.reshape(1, 48, 48, 1)

    # Predict emotions using the pre-trained model
    preds = model.predict(reshaped_face)[0]
    emotion_idx = preds.argmax()
    emotion = emotion_labels[emotion_idx]

    # Draw rectangle around face and label with predicted emotion
    cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 0, 255), 2)
    cv2.putText(frame, emotion, (x, y - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.9, (0, 0, 255), 2)

# Display the resulting frame
cv2.imshow('Real-time Emotion Detection', frame)

# Press 'q' to exit
if cv2.waitKey(1) & 0xFF == ord('q'):
    break

