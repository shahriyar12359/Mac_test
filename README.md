
import pyttsx3
import speech_recognition as sr
import pywhatkit
import os

engine = pyttsx3.init()
recognizer = sr.Recognizer()

def speak(text):
engine.say(text)
engine.runAndWait()

def listen():
with sr.Microphone() as source:
print("Listening...")
recognizer.adjust_for_ambient_noise(source)
audio = recognizer.listen(source)
command = ""
try:
command = recognizer.recognize_google(audio)
print("You said: " + command)
except sr.UnknownValueError:
print("Sorry, I didn't get that.")
except sr.RequestError:
print("Sorry, I couldn't request results; check your network connection.")
return command

def main():
speak("Hello, I'm Jarvis. How can I assist you?")
while True:
command = listen().lower()
if "summarize news" in command:
speak("Sure, summarizing today's news headlines.")
# Fetching exactly 50 words of news
news_summary = pywhatkit.info("Today's news headlines", lines=1)
news_summary = ' '.join(news_summary.split()[:50])
speak(news_summary)
elif "find calculator" in command:
speak("Sure, let's calculate.")
speak("Please provide the mathematical expression.")
calculation = listen()
try:
result = eval(calculation)
speak(f"The result of {calculation} is {result}")
except Exception as e:
speak("I couldn't perform the calculation. Please try again.")
elif "exit" in command:
speak("Goodbye!")
break
else:
speak("I'm not sure how to do that. Please give me a valid command.")

if __name__ == "__main__":
main()
