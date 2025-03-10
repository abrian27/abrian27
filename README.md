- 👋 Hi, I’m @abrian27
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
abrian27/abrian27 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import openai
import speech_recognition as sr
from gtts import gTTS
import os

# Configura tu clave de API de OpenAI aquí
openai.api_key = "TU_API_KEY"

# Función para convertir texto a voz
def hablar(texto):
    tts = gTTS(text=texto, lang="es")
    tts.save("respuesta.mp3")
    os.system("start respuesta.mp3")  # Usa "afplay" en Mac o "mpg321" en Linux

# Función para escuchar y reconocer la voz
def escuchar():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("🎙️ Escuchando...")
        recognizer.adjust_for_ambient_noise(source)
        audio = recognizer.listen(source)
    
    try:
        texto = recognizer.recognize_google(audio, language="es-ES")
        print(f"👤 Tú: {texto}")
        return texto
    except sr.UnknownValueError:
        print("❌ No entendí lo que dijiste.")
        return ""
    except sr.RequestError:
        print("❌ Error en la conexión.")
        return ""

# Función para generar una respuesta con OpenAI
def responder(pregunta):
    try:
        respuesta = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",
            messages=[{"role": "user", "content": pregunta}]
        )
        mensaje = respuesta["choices"][0]["message"]["content"]
        return mensaje
    except Exception as e:
        print(f"❌ Error al generar respuesta: {e}")
        return "Lo siento, hubo un problema generando la respuesta."

# Bucle principal para la conversación
def iniciar_asistente():
    print("🤖 Bryan Lopez: Hola, soy Bryan Lopez. ¿En qué puedo ayudarte hoy?")
    hablar("Hola, soy Bryan Lopez. ¿En qué puedo ayudarte hoy?")

    while True:
        entrada = escuchar()
        if entrada.lower() in ["salir", "adiós", "terminar"]:
            print("👋 Bryan Lopez: Hasta luego.")
            hablar("Hasta luego.")
            break
        
        if entrada != "":
            respuesta = responder(entrada)
            print(f"🤖 Bryan Lopez: {respuesta}")
            hablar(respuesta)

# Iniciar el asistente
if __bryan__ == "__main__":
    iniciar_asistente()
