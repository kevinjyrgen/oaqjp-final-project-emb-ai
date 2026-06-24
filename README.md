# Emotion Detection

A Flask web application that analyzes the emotional content of any text and returns a breakdown of five emotions plus the dominant one. Built on IBM Watson NLP's emotion-prediction model.

Capstone project for the **IBM AI Developer Professional Certificate** (course: *Developing AI Applications with Python and Flask*).

---

## What it does

Given a sentence, the app calls IBM Watson's emotion-detection endpoint and returns the confidence scores for **anger, disgust, fear, joy,** and **sadness**, then reports which emotion dominates.

**Example**

Input:
```
I love this new place, it makes me so happy.
```

Output:
```
For the given statement, the system response is 'anger': 0.0048, 'disgust': 0.0010,
'fear': 0.0085, 'joy': 0.9680, 'sadness': 0.0096. The dominant emotion is joy.
```

---

## Architecture

```
oaqjp-final-project-emb-ai/
├── EmotionDetection/
│   ├── __init__.py              # exposes emotion_detector
│   └── emotion_detection.py     # core logic: calls Watson NLP, parses response
├── templates/
│   └── index.html               # web front-end
├── static/
│   └── mywebscript.js           # client-side request handling
├── server.py                    # Flask app + routes
├── test_emotion_detection.py    # unit tests (all five emotions)
└── LICENSE
```

The design separates concerns deliberately:

- **`EmotionDetection`** is a standalone, importable package — the detection logic has no Flask dependency, so it can be reused or tested in isolation.
- **`server.py`** is a thin Flask layer that handles routing, input validation, and response formatting.
- **`test_emotion_detection.py`** verifies the detector against a labelled example for each emotion.

---

## How it works

`emotion_detector()` sends the input text to the Watson NLP `EmotionPredict` service, parses the JSON response, and computes the dominant emotion as the label with the highest confidence score:

```python
dominant_emotion = max(emotions, key=emotions.get)
```

Blank input or a `400` response from the API returns a structured result with `None` values, which the server translates into a friendly `"Invalid text! Please try again."` message rather than crashing.

---

## Running locally

**Requirements:** Python 3.x, `flask`, `requests`

```bash
# install dependencies
pip install flask requests

# start the server
python server.py
```

Then open **http://localhost:5000** in your browser, or hit the endpoint directly:

```
http://localhost:5000/emotionDetector?textToAnalyze=I am thrilled with the result
```

---

## Running the tests

```bash
python -m unittest test_emotion_detection.py
```

The suite asserts the correct dominant emotion for a representative sentence of each type (joy, anger, disgust, sadness, fear).

---

## Notes

- The Watson NLP endpoint used here is hosted on IBM's Skills Network environment and is intended for the course context. To run against a different deployment, update the `url` in `emotion_detection.py`.
- Code passes `pylint` with a clean score.

---

## License

Apache-2.0 — see [LICENSE](LICENSE).
