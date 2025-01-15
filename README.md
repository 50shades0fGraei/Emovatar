flutter create emotion_avatar_app
cd emotion_avatar_app
npm install -g react-native-cli
npx react-native init EmotionAvatarApp
npx react-native init EmotionAvatarApp
cd EmotionAvatarApp
dependencies:
  http: ^0.13.3
  flutter_dotenv: ^5.0.2
import 'package:http/http.dart' as http;
import 'dart:convert';
import 'package:flutter_dotenv/flutter_dotenv.dart';
dependencies:
  http: ^0.13.3
  flutter_dotenv: ^5.0.2import 'package:http/http.dart' as http;
import 'dart:convert';
import 'package:flutter_dotenv/flutter_dotenv.dart';

Future<String> detectEmotion(String text) async {
  final apiKey = dotenv.env['HUGGING_FACE_API_KEY'];
  final response = await http.post(
    Uri.parse('https://api-inference.huggingface.co/models/mrm8488/t5-base-finetuned-emotion'),
    headers: {
      'Authorization': 'Bearer $apiKey',
      'Content-Type': 'application/json',
    },
    body: jsonEncode({'inputs': text}),
  );

  if (response.statusCode == 200) {
    final result = jsonDecode(response.body);
    return result[0]['label'];
  } else {
    throw Exception('Failed to detect emotion');
  }
}npm install axios react-native-dotenvHUGGING_FACE_API_KEY=your_api_keymodule.exports = {
  presets: ['module:metro-react-native-babel-preset'],
  plugins: [
    ['module:react-native-dotenv'],
  ],
};import axios from 'axios';
import { HUGGING_FACE_API_KEY } from '@env';

const detectEmotion = async (text) => {
  try {
    const response = await axios.post(
      'https://api-inference.huggingface.co/models/mrm8488/t5-base-finetuned-emotion',
      { inputs: text },
      { headers: { Authorization: `Bearer ${HUGGING_FACE_API_KEY}` } }
    );
    return response.data[0].label;
  } catch (error) {
    console.error('Failed to detect emotion', error);
    throw error;
  }
};

export default detectEmotion;import 'package:flutter/material.dart';
import 'package:flutter_dotenv/flutter_dotenv.dart';
import 'detect_emotion.dart'; // assumes detectEmotion function is in this file

void main() async {
  await dotenv.load();
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: EmotionAvatarScreen(),
    );
  }
}

class EmotionAvatarScreen extends StatefulWidget {
  @override
  _EmotionAvatarScreenState createState() => _EmotionAvatarScreenState();
}

class _EmotionAvatarScreenState extends State<EmotionAvatarScreen> {
  String _emotion = 'neutral';

  void _updateEmotion(String text) async {
    final emotion = await detectEmotion(text);
    setState(() {
      _emotion = emotion;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Emotion Avatar')),
      body: Column(
        children: [
          Image.asset('assets/$_emotion.png'), // assumes you have images named after emotions
          TextField(
            onSubmitted: _updateEmotion,
            decoration: InputDecoration(hintText: 'Enter text to analyze emotion'),
          ),
        ],
      ),
    );
  }
}import React, { useState } from 'react';
import { View, TextInput, Image, StyleSheet, Text } from 'react-native';
import detectEmotion from './detectEmotion'; // assumes detectEmotion function is in this file

const EmotionAvatarScreen = () => {
  const [emotion, setEmotion] = useState('neutral');

  const updateEmotion = async (text) => {
    try {
      const detectedEmotion = await detectEmotion(text);
      setEmotion(detectedEmotion);
    } catch (error) {
      console.error('Failed to detect emotion', error);
    }
  };

  return (
    <View style={styles.container}>
      <Image source={{ uri: `assets/${emotion}.png` }} style={styles.avatar} />
      <TextInput
        style={styles.input}
        placeholder="Enter text to analyze emotion"
        onSubmitEditing={(event) => updateEmotion(event.nativeEvent.text)}
      />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 16,
  },
  avatar: {
    width: 100,
    height: 100,
    marginBottom: 16,
  },
  input: {
    height: 40,
    borderColor: 'gray',
    borderWidth: 1,
    paddingHorizontal: 8,
  },
});

export default EmotionAvatarScreen;
Future<String> detectEmotion(String text) async {
  final apiKey = dotenv.env['HUGGING_FACE_API_KEY'];
  final response = await http.post(
    Uri.parse('https://api-inference.huggingface.co/models/mrm8488/t5-base-finetuned-emotion'),
    headers: {
      'Authorization': 'Bearer $apiKey',
      'Content-Type': 'application/json',
    },
    body: jsonEncode({'inputs': text}),
  );

  if (response.statusCode == 200) {
    final result = jsonDecode(response.body);
    return result[0]['label'];
  } else {
    throw Exception('Failed to detect emotion');
  }
}
npm install axios react-native-dotenvnpm install axios react-native-dotenvnpm install axios react-native-dotenv

pip install fer

import cv2
from fer import FER

# Load the pre-trained face detection model
face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')
# Initialize the emotion detector
detector = FER()

# Start video capture
cap = cv2.VideoCapture(0)

while True:
    # Read frame from the camera
    ret, frame = cap.read()

    # Convert to grayscale
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # Detect faces
    faces = face_cascade.detectMultiScale(gray, scaleFactor=1.1, minNeighbors=5, minSize=(30, 30))

    # Analyze emotions on detected faces
    for (x, y, w, h) in faces:
        face = frame[y:y+h, x:x+w]
        emotion, score = detector.top_emotion(face)
        cv2.putText(frame, f'{emotion} ({score:.2f})', (x, y - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.9, (255, 0, 0), 2)
        cv2.rectangle(frame, (x, y), (x+w, y+h), (255, 0, 0), 2)

    # Display the result
    cv2.imshow('Emotion Detection', frame)

    # Break the loop on 'q' key press
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# Release the capture and close windows
cap.release()
cv2.destroyAllWindows()

pip install pygame


import pygame
import sys

# Initialize Pygame
pygame.init()

# Set up the display
screen = pygame.display.set_mode((800, 600))
pygame.display.set_caption("Avatar Display")

# Load the avatar image
avatar_image = pygame.image.load("avatar.png")

# Main loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Clear the screen
    screen.fill((255, 255, 255))

    # Draw the avatar
    screen.blit(avatar_image, (100, 100))

    # Update the display
    pygame.display.flip()

# Quit Pygame
pygame.quit()
sys.exit()# Emovatar
An avatar that acts out the emotional state of person you are contacting
