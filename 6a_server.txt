"""
Servidor Flask para la aplicación de Detección de Emociones.
Proporciona una interfaz web para analizar textos y mostrar
puntuaciones de alegría, ira, miedo, asco y tristeza.
"""
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask("Emotion Detector")

@app.route("/emotionDetector")
def emo_detector():
    """
    Ruta que recibe el texto de la interfaz, llama al detector de emociones
    y devuelve la respuesta formateada para el usuario.
    """
    # Recuperar el texto a analizar desde los argumentos de la URL
    text_to_analyze = request.args.get('textToAnalyze')

    # Pasar el texto a la función de detección de emociones del paquete
    response = emotion_detector(text_to_analyze)

    # Extraer los valores del diccionario de respuesta
    anger = response['anger']
    disgust = response['disgust']
    fear = response['fear']
    joy = response['joy']
    sadness = response['sadness']
    dominant_emotion = response['dominant_emotion']

    # Retornar la respuesta con el formato exacto solicitado en los requerimientos
    return (
        f"For the given statement, the system response is 'anger': {anger}, "
        f"'disgust': {disgust}, 'fear': {fear}, 'joy': {joy} and 'sadness': {sadness}. "
        f"The dominant emotion is {dominant_emotion}."
    )

@app.route("/")
def render_index_page():
    """
    Ruta para renderizar la página principal de la aplicación.
    """
    return render_template('index.html')

if __name__ == "__main__":
    # Desplegar la aplicación en localhost:5000
    app.run(host="0.0.0.0", port=5000)