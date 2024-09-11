import speech_recognition as sr
import webbrowser
import pyttsx3
import musicLibrary
import requests
from openai import OpenAI, completions

recognizer=sr.Recognizer()#helps in speech recognition
engine=pyttsx3.init()
newsAPI="823bfe55d3ce4a61aa5641c7313a3f63"

def speak(Text):
    engine.say(Text)
    engine.runAndWait()

def aiprocess(command):
    client=OpenAI(api_key="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"
      )
    completion = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[
            {"role": "system", "content": "You are a helpful assistant."},
            {
                "role": "user",
                "content": command}
        ]
    )

    return (completions.choices[0].message.content)

def processCommand(c):
    if c.lower()=="open chrome":
        webbrowser.open("https://google.com")
    elif c.lower()=="open youtube":
        webbrowser.open("https://youtube.com")
    elif c.lower()=="open linkedin":
        webbrowser.open("https://linkedin.com")
    elif c.lower()=="open github":
        webbrowser.open("https://github.com")
    elif "play" in c.lower():
        song=c.lower().split(" ")[1]#splits comman in play and song_name and follow index 1,i.e. song_name
        link=musicLibrary.music[song]
        webbrowser.open(link)
    elif "news" in c.lower():
        r=requests.get("https://newsapi.org/v2/top-headlines?country=in&apiKey={newsapi}")
        if r.status_code == 200:
            data=r.json()
            #extract the articles
            articles=data.get('articles',[])
            #print the headlines
            for article in articles:
                speak(article['title'])
    else:
        output=aiprocess(c)
        speak(output)

if __name__ == "__main__":
    speak("Power on")
    while True:
#Listen for the wake word "alexa"
        r=sr.Recognizer()
        print("Recognizing...")
        
        try:
            with sr.Microphone() as source:
                print("Say something")
                audio=r.listen(source,timeout=2,phrase_time_limit=1)
            wake_word=r.recognize_google(audio)
            if (wake_word.lower() == "alexa"):
                speak("Yes, I am listening")
                with sr.Microphone() as source:
                    print("Alexa active")
                    audio=r.listen(source)
                    command=r.recognize_google(audio)
                    processCommand(command)
        except Exception as e:
            print("Error:",e)
