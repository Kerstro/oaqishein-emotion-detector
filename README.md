# oaqishein-emotion-detector

This is an AI-based web application - Emotion Detector - developed using Watson NLP library and Flask. The application detects emotions from text input including anger, disgust, fear, joy, and sadness, and returns the dominant emotion.

https://github.com/Kerstro/oaqishein-emotion-detector/blob/main/README.md

import json
import requests

def emotion_detector(text_to_analyse):
    if text_to_analyse is None or text_to_analyse.strip() == '':
        return {'anger': None, 'disgust': None, 'fear': None, 'joy': None, 'sadness': None, 'dominant_emotion': None}
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    myobj = { "raw_document": { "text": text_to_analyse } }
    header = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    response = requests.post(url, json = myobj, headers=header)
    if response.status_code == 400:
        return {'anger': None, 'disgust': None, 'fear': None, 'joy': None, 'sadness': None, 'dominant_emotion': None}
    formatted_response = json.loads(response.text)
    emotions = formatted_response['emotionPredictions'][0]['emotion']
    anger_score = emotions['anger']
    disgust_score = emotions['disgust']
    fear_score = emotions['fear']
    joy_score = emotions['joy']
    sadness_score = emotions['sadness']
    emotion_dict = {'anger': anger_score, 'disgust': disgust_score, 'fear': fear_score, 'joy': joy_score, 'sadness': sadness_score}
    dominant_emotion = max(emotion_dict, key=emotion_dict.get)
    return {'anger': anger_score, 'disgust': disgust_score, 'fear': fear_score, 'joy': joy_score, 'sadness': sadness_score, 'dominant_emotion': dominant_emotion}

    >>> from EmotionDetection import emotion_detector
>>> emotion_detector('I am so happy I am doing this')
{'anger': 0.006274283, 'disgust': 0.0028019438, 'fear': 0.009469712, 'joy': 0.9787002, 'sadness': 0.008294394, 'dominant_emotion': 'joy'}

import json
import requests

def emotion_detector(text_to_analyse):
    if text_to_analyse is None or text_to_analyse.strip() == '':
        return {'anger': None, 'disgust': None, 'fear': None, 'joy': None, 'sadness': None, 'dominant_emotion': None}
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    myobj = { "raw_document": { "text": text_to_analyse } }
    header = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    response = requests.post(url, json = myobj, headers=header)
    if response.status_code == 400:
        return {'anger': None, 'disgust': None, 'fear': None, 'joy': None, 'sadness': None, 'dominant_emotion': None}
    formatted_response = json.loads(response.text)
    emotions = formatted_response['emotionPredictions'][0]['emotion']
    anger_score = emotions['anger']
    disgust_score = emotions['disgust']
    fear_score = emotions['fear']
    joy_score = emotions['joy']
    sadness_score = emotions['sadness']
    emotion_dict = {'anger': anger_score, 'disgust': disgust_score, 'fear': fear_score, 'joy': joy_score, 'sadness': sadness_score}
    dominant_emotion = max(emotion_dict, key=emotion_dict.get)
    return {'anger': anger_score, 'disgust': disgust_score, 'fear': fear_score, 'joy': joy_score, 'sadness': sadness_score, 'dominant_emotion': dominant_emotion}

    >>> from EmotionDetection import emotion_detector
>>> emotion_detector('I am so happy I am doing this')
{'anger': 0.006274283, 'disgust': 0.0028019438, 'fear': 0.009469712, 'joy': 0.9787002, 'sadness': 0.008294394, 'dominant_emotion': 'joy'}
>>> emotion_detector('I am really mad about this')
{'anger': 0.7527173, 'disgust': 0.09670219, 'fear': 0.07609832, 'joy': 0.005400908, 'sadness': 0.062298283, 'dominant_emotion': 'anger'}

https://github.com/Kerstro/oaqishein-emotion-detector/blob/main/EmotionDetection/__init__.py

>>> import EmotionDetection
>>> EmotionDetection.emotion_detector('I am feeling great')
{'anger': 0.0077027573, 'disgust': 0.0049132804, 'fear': 0.011669661, 'joy': 0.9574928, 'sadness': 0.016596649, 'dominant_emotion': 'joy'}

import unittest
from EmotionDetection.emotion_detection import emotion_detector

class TestEmotionDetector(unittest.TestCase):
    def test_joy_emotion(self):
        result = emotion_detector('I am glad this happened')
        self.assertEqual(result['dominant_emotion'], 'joy')

    def test_anger_emotion(self):
        result = emotion_detector('I am really mad about this')
        self.assertEqual(result['dominant_emotion'], 'anger')

    def test_disgust_emotion(self):
        result = emotion_detector('I feel disgusted just hearing about this')
        self.assertEqual(result['dominant_emotion'], 'disgust')

    def test_sadness_emotion(self):
        result = emotion_detector('I am so sad about this')
        self.assertEqual(result['dominant_emotion'], 'sadness')

    def test_fear_emotion(self):
        result = emotion_detector('I am really afraid that this will happen')
        self.assertEqual(result['dominant_emotion'], 'fear')

if __name__ == '__main__':
    unittest.main()

#
python -m pytest test_emotion_detection.py -v
=============================== test session starts ================================
platform linux -- Python 3.11.9, pytest-8.3.3, pluggy-1.5.0
rootdir: /home/project
collected 5 items

test_emotion_detection.py::TestEmotionDetector::test_anger_emotion PASSED  [ 20%]
test_emotion_detection.py::TestEmotionDetector::test_disgust_emotion PASSED  [ 40%]
test_emotion_detection.py::TestEmotionDetector::test_fear_emotion PASSED  [ 60%]
test_emotion_detection.py::TestEmotionDetector::test_joy_emotion PASSED  [ 80%]
test_emotion_detection.py::TestEmotionDetector::test_sadness_emotion PASSED  [100%]

================================ 5 passed in 12.57s ================================

#
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask("Emotion Detector")

@app.route("/emotionDetector")
def sent_detector():
    text_to_analyse = request.args.get('textToAnalyse')
    response = emotion_detector(text_to_analyse)
    if response['dominant_emotion'] is None:
        return "Invalid text! Please try again!"
    anger = response['anger']
    disgust = response['disgust']
    fear = response['fear']
    joy = response['joy']
    sadness = response['sadness']
    dominant_emotion = response['dominant_emotion']
    return (f"For the given statement, the system response is "
            f"'anger': {anger}, 'disgust': {disgust}, "
            f"'fear': {fear}, 'joy': {joy} and "
            f"'sadness': {sadness}. "
            f"The dominant emotion is {dominant_emotion}.")

@app.route("/")
def render_index_page():
    return render_template('index.html')

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)

#
6b_deployment_test.png.

#
Question 13 — Score: 1/1 ✅
Task 7, Activity 2: server.py handling blank input errors.

Answer: (same code as Q10 — already returns "Invalid text! Please try again!" when dominant_emotion is None)

#
Question 14 — Score: 0/1
Task 7, Activity 3: Upload image 7c_error_handling_interface.png.

Answer: No answer provided (requires screenshot from running lab environment).

#
"""Flask server for Emotion Detector application."""
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask("Emotion Detector")

@app.route("/emotionDetector")
def sent_detector():
    """Detect emotions from a given text and return formatted results."""
    ...  # same body as Q10

@app.route("/")
def render_index_page():
    """Render the main index page."""
    return render_template('index.html')

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)


#
pylint server.py
************* Module server

-------------------------------------------------------------------
Your code has been rated at 10.00/10 (previous run: 9.39/10, +0.61)
