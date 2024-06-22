from flask import Flask, request, jsonify, send_from_directory
import speech_recognition as sr
import os

app = Flask(__name__)

@app.route('/')
def index():
    return send_from_directory('.', 'video.html')

@app.route('/static/<path:path>')
def static_files(path):
    return send_from_directory('static', path)

@app.route('/generate_subtitles', methods=['POST'])
def generate_subtitles():
    recognizer = sr.Recognizer()
    audio_file = "static/your_video_audio.wav"  # Ensure this file exists or extract from video

    try:
        with sr.AudioFile(audio_file) as source:
            audio = recognizer.record(source)
            text = recognizer.recognize_google(audio, language="ja-JP")

            # Generate SRT content
            srt_content = generate_srt(text)

            srt_path = 'static/generated_subtitles.srt'
            with open(srt_path, 'w') as srt_file:
                srt_file.write(srt_content)
            
            return jsonify(subtitle_url=f'/static/{os.path.basename(srt_path)}')
    except Exception as e:
        return jsonify(error=str(e)), 500

def generate_srt(text):
    # Simplified SRT generation (needs improvement for real usage)
    lines = text.split('\n')
    srt_content = ""
    for i, line in enumerate(lines):
        srt_content += f"{i+1}\n00:00:{i:02d},000 --> 00:00:{i+2:02d},000\n{line}\n\n"
    return srt_content

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
