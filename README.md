import speech_recognition as sr
import os
import datetime
import webbrowser
import time
import tempfile # Yeh nayi line add karein

def speak(audio):
    try:
        print("NOVA is speaking...")
        # Google Text-to-Speech se audio file banayein
        tts = gtts.gTTS(text=audio, lang='hi', slow=False)
        
        # Ek surakshit temporary file path banayein
        temp_file_path = os.path.join(tempfile.gettempdir(), "temp.mp3")
        tts.save(temp_file_path)
        
        # Audio file ko system command se bajayein
        os.system(f"start {temp_file_path}")
        
        # Audio file bajne tak ruk jayein
        time.sleep(2) 
        
        # Temporary file hata dein
        os.remove(temp_file_path)
        
    except Exception as e:
        print(f"Error in speaking: {e}")

def takeCommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("NOVA is listening...")
        r.pause_threshold = 1
        audio = r.listen(source)

    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language='hi-in')
        print(f"User said: {query}\n")
    except Exception as e:
        print("Maaf kijiye, main yeh nahi samajh paya...")
        return "None"
    return query

if __name__ == "__main__":
    time.sleep(10) # 10 seconds ka delay jodein
    speak("Namaste! Main NOVA hoon. Kaise madad karoon?")

    while True:
        query = takeCommand().lower()

        if "hello" in query or "namaste" in query:
            speak("Namaste! Main thik hoon, aap kaise hain?")

        elif "samay kya hai" in query or "time batao" in query:
            strTime = datetime.datetime.now().strftime("%H:%M")
            speak(f"Sir, abhi samay {strTime} hai.")

        elif "google par search karo" in query:
            speak("Kya search karna hai, sir?")
            search_query = takeCommand().lower()
            url = f"https://www.google.com/search?q={search_query}"
            webbrowser.open(url)
            speak(f"Aapki search {search_query} google par khol di gayi hai.")

        elif "tum kaun ho" in query:
            speak("Main aapka personal voice assistant NOVA hoon.")
            
        elif "bye" in query:
            speak("Alvida! Aapka din shubh ho.")
            break
        
        else:
            speak("Maaf kijiye, main yeh nahi samajh paya. Kya aap dobara bol sakte hain?")
