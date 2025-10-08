# Hydra-
import speech_recognition as sr
import pyttsx3
import re
import webbrowser
import datetime
import os

def clean_text(text):
    return re.sub(r'[^\x00-\x7F]+', '', text)  # remove non-ascii chars

def speak(text):
    clean = clean_text(text)
    engine = pyttsx3.init()
    engine.say(clean)
    engine.runAndWait()

def listen():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        audio = recognizer.listen(source)
    try:
        command = recognizer.recognize_google(audio)
        print(f"You said: {command}")
        return command.lower()
    except:
        speak("Sorry, I didn't catch that.")
        return ""

def execute_command(command):
    if "time" in command:
        now = datetime.datetime.now().strftime("%H:%M")
        speak(f"The time is {now}")
    elif "open google" in command:
        speak("Opening Google")
        webbrowser.open("https://www.google.com")
    elif "open youtube" in command:
        speak("Opening YouTube")
        webbrowser.open("https://www.youtube.com")
    elif "open notepad" in command:
        speak("Opening Notepad")
        os.system("notepad")
    elif "exit" in command or "stop" in command:
        speak("Goodbye!")
        return False
    else:
        speak("I can perform basic tasks like telling time or opening websites/apps.")
    return True

def main():
    speak("Hello! I am your assistant. How can I help you?")
    running = True
    while running:
        command = listen()
        if command:
            running = execute_command(command)

if __name__ == "__main__":
    main()
