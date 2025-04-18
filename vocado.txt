import speech_recognition as sr
import pyttsx3

# Initialize
todo_list = []
listener = sr.Recognizer()
engine = pyttsx3.init()

# Speak function
def speak(text):
    engine.say(text)
    engine.runAndWait()

# Get voice input
def get_command():
    try:
        with sr.Microphone() as source:
            print("🎙 Listening...")   
            audio = listener.listen(source)
            command = listener.recognize_google(audio)
            command = command.lower()
            print(f"🗣 You said: {command}")
            return command
    except:
        print("❌ Voice not recognized.")
        speak("Sorry, I did not understand.")
        return ""

# Save list to file
def save_list():
    with open("todo_list.txt", "w") as file:
        for task in todo_list:
            file.write(task + "\n")
    print("📄 To-do list saved to todo_list.txt")

# Welcome
speak("Welcome to your voice-controlled to-do list!")

# Main loop
while True:
    command = get_command()

    # Add task
    if 'add' in command:
        task = command.replace('add', '').strip()
        if task:
            todo_list.append(task)
            speak(f"Added {task} to your list.")
        else:
            speak("What should I add? Please say it again.")

    # Display tasks
    elif 'display' in command:
        print("📋 Display command triggered")
        if todo_list:
            print("🧠 Current list:", todo_list)
            speak("Here are your tasks:")
            for task in todo_list:
                speak(task)
        else:
            speak("Your to-do list is empty.")

    # Remove task
    elif 'remove' in command or 'delete' in command:
        task = command.replace('remove', '').replace('delete', '').strip()
        if task in todo_list:
            todo_list.remove(task)
            speak(f"Removed {task}")
        else:
            speak(f"{task} not found in the list.")

    # Save manually
    elif 'save' in command:
        save_list()
        speak("Your to-do list has been saved to a file.")

    # Exit and auto-save
    elif 'exit' in command or 'stop' in command:
        save_list()
        speak("Your list has been saved. Bye! Have a productive day!")
        break

    # Smart replies
    elif 'hello' in command:
        speak("Hello! Ready to manage your tasks.")

    elif 'how are you' in command:
        speak("I am great! Thanks for asking. How are you?")

    elif 'thank you' in command or 'thanks' in command:
        speak("You're welcome!")

    elif 'what can you do' in command:
        speak("I can help you add, display, or remove your to-do tasks using your voice.")

    elif 'your name' in command:
        speak("I am your voice assistant, your to-do list partner.")

    elif 'repeat my list' in command:
        if todo_list:
            speak("Sure, your list is:")
            for task in todo_list:
                speak(task)
        else:
            speak("You have no tasks yet.")

    # Unknown command
    elif command != "":
        speak("Sorry, I did not understand.")