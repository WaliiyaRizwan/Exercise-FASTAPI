# FastAPI Video Analysis

This project is a FastAPI application that allows users to upload video files, extract audio, transcribe it using OpenAI's Whisper model, and analyze the transcriptions for the presence of specified "bad words." The project provides a REST API for handling video uploads and analyzing the audio.

## Features

- **Video Upload:** Upload one or more video files for analysis.
- **Audio Extraction:** Extract audio from the uploaded videos.
- **Transcription:** Transcribe the extracted audio using OpenAI's Whisper model.
- **Bad Word Analysis:** Count occurrences of specified "bad words" in the transcriptions.
- **Error Handling:** Proper error handling for file validation, transcription, and other potential errors.

## Requirements

- Python 3.10 or later
- FastAPI
- MoviePy
- Requests
- git+https://github.com/openai/whisper.git 
- ffmpeg

## Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/yourusername/fastapi-video-analysis.git
   cd fastapi-video-analysis

## Working 
1. Run the App:
   ```bash
   python server.py

   http://127.0.0.1:5001/api/video/uploadvideo
   
2. Video Uploading and Validation:
   ```
   for file in files:
        try:
            # Validate that the uploaded file has a video extension
            if not VideoValidation(file.filename):
                raise HTTPException(status_code=400, detail=f"Invalid file type. Please upload a video                file. ({file.filename})")

            # Save the video file temporarily
            video_path = file.filename
            with open(video_path, "wb") as buffer:
                buffer.write(file.file.read())
   ```
   # Validation
   ```
   def VideoValidation(file_name: str) -> bool:
       video_extensions = {".mp4", ".avi", ".mkv", ".mov", ".wmv"} 
       _, file_extension = os.path.splitext(file_name)
       return file_extension.lower() in video_extensions
   ```

3. Audio Extraction:
   ```
   def extract_audio(video_path: str, output_path: str):
       video_clip = VideoFileClip(video_path)
       audio_clip = video_clip.audio
       audio_clip.write_audiofile(output_path)
       video_clip.close()
       return output_path
   ```
    
4. Transcription:

   ```
   model = whisper.load_model("base", device=DEVICE)

   def extract_text(audio_file):
       result = model.transcribe(audio_file)
       return result["text"]
   ```
5. Bad Word Analysis:

   ```
   bad_words = ["tomaco", "tarot", "tell", "ask", "bite", "orange", "carrot"]
   bad_word_counts = {word: text.lower().count(word) for word in bad_words}
   ```
   
