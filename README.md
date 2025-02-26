# AI-Desktop-agent

import pyttsx3  # Text-to-Speech
import speech_recognition as sr  # Speech Recognition
import datetime  # Time & Date
import wikipedia  # Wikipedia Search
import webbrowser  # Open Websites
import os  # OS Commands
import pyjokes  # Jokes Library

# Initialize the speech engine
engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[0].id)

def speak(audio):
    """Speaks the given text."""
    engine.say(audio)
    engine.runAndWait()

def wishMe():
    """Greets the user based on the time of day."""
    hour = datetime.datetime.now().hour
    if hour < 12:
        speak("Good Morning!")
    elif hour < 18:
        speak("Good Afternoon!")
    else:
        speak("Good Evening!")

    speak("I am Jarvis, sir. How can I assist you?")

def takecommand():
    """Listens to the user's voice and returns the recognized text."""
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1
        audio = r.listen(source)

    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language='en-in')
        print(f"User said: {query}\n")
        return query.lower()

    except Exception:
        print("Could not understand. Please say that again...")
        return "None"

if __name__ == "__main__":
    wishMe()
    
    while True:  # Continuous listening loop
        query = takecommand()

        # Wikipedia Search
        if "wikipedia" in query:
            speak("Searching Wikipedia...")
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, sentences=2)
            speak("According to Wikipedia:")
            print(results)
            speak(results)

        # Open Websites
        elif 'open youtube' in query:
            webbrowser.open("youtube.com")
        elif 'open google' in query:
            webbrowser.open("google.com")
        elif 'stackoverflow' in query:
            webbrowser.open("stackoverflow.com")
        elif 'open spotify' in query:
            webbrowser.open("spotify.com")

        # Play Music
        elif 'play music' in query:
            music_dir = 'D:\\Non critical\\songs\\favorite songs2'
            songs = os.listdir(music_dir)
            os.startfile(os.path.join(music_dir, songs[0]))

        # Tell Time
        elif 'the time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            speak(f"Sir, the time is {strTime}")

        # Open VS Code
        elif 'open code' in query:
            codepath = "C:\\Users\\HP\\Downloads\\VSCodeUserSetup-x64-1.96.2.exe"
            os.startfile(codepath)

        # Tell a Joke
        elif 'tell me a joke' in query:
            joke = pyjokes.get_joke()
            speak(joke)

        # Shutdown PC
        elif 'shutdown' in query:
            speak("Shutting down the system in 5 seconds.")
            os.system("shutdown /s /t 5")
            break


