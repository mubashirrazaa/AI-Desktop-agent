# AI-Desktop-agent

import pyttsx3 #pip install pyttsx3
import speech_recognition as sr #pip install speech_recognition
import datetime
import wikipedia #pip install wikipedia 
import webbrowser
import os
import time
import pyautogui
import pyjokes



engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
print(voices[0].id)
engine.setProperty('voice',voices[0].id)

def speak(audio):
    engine.say(audio)
    engine.runAndWait()

def wishMe():
    hour = int(datetime.datetime.now().hour)
    if hour>=0 and hour<12:
        speak("Good Morning!")

   elif hour>=12 and hour<18:
        speak("Good Afternoon!")

 else:
        ("Good Evening!")


speak("I am a jarvis sir. Please tell me how may i help you")


def takecommand():
    #It takes microphone from the user and returns string output

   r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1
        audio = r.listen(source)
    try:
        print("recognizing...")
        query = r.recognize_google(audio, language='en-in')
      print(f"User said: {query}\n")  

   except Exception as e:
        #print(e)
        print("say that again please...")
        return "None"
    return query


if __name__ == "__main__":
    wishMe()
    #while True:
    if 1:
        query = takecommand().lower()
         #logic for executing tasks based on query
        if "wikipedia" in query:
             speak("speak wikipedia...")
             query = query.replace("wikipedia", "")
             results = wikipedia.summary(query,sentences=2)
             speak("according to wikipedia")
             print(results)
             speak(results)
        elif 'open youtube' in query:
            webbrowser.open("youtube.com")
            elif 'open google' in query:
            webbrowser.open("google.com")

   elif 'stackoverflow' in query:
            webbrowser.open("stackoverflow.com")
       elif 'play music' in query:
            music_dir = 'D:\\Non critical\\songs\\favorite songs2'
            songs = os.listdir(music_dir)
            print(songs)
            os.startfile(os.path.join(music_dir, songs[0]))
      elif 'open spotify' in query:
           webbrowser.open("spotify.com")
    elif 'the time' in query:
              strTime = datetime.datetime.now().strftime("%H:%M:%S") 
              speak(f"sir, the time is {strTime}") 
       elif 'open code' in query:
            codepath = "C:\\Users\\HP\\Downloads\\VSCodeUserSetup-x64-1.96.2.exe"
            os.startfile(codepath)
       elif 'tell me a joke' in query:
         joke = pyjokes.get_joke()
         speak(joke)
       elif 'shutdown' in query:
         os.system("shutdown /s /t 1")
