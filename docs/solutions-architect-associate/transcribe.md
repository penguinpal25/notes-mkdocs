# Amazon Transcribe

- Automatically convert speech to text (convert audio to text)
- Uses deep learning process called automatic speech recognition (ASR) to convert speech to text quickly and accurately
- Automatically remove personally identifiable information (PII) using Redaction (such as age, name...)
- Supports automatic language identification for multi-lingual audio
- 
**Use cases**
- transcribe customer service calls
- automate closed captioning and subtitles
- generate metadata for media assets to create a fully searchable archive
- document clinical conversations into electronic health record (EHR) systems for analysis (HIPAA eligible and trained to understand medical terminology)

## How Amazon Transcribe works
Amazon Transcribe converts speech to text. A basic transcription request produces a transcript data that contains data about the transcribed content, including confidence scores and timestamps for each word or punctuation mark.
Transcription method can be separated in to two main categories
- Batch transcription jobs: Transcribe media files that have been uploaded to S3 bucket
- Streaming transcriptions: Transcribe media streams in real time

![enter image description here](https://d1.awsstatic.com/product-marketing/product-page-diagram_Amazon-Transcribe-Call-Analytics_HIW-2x.2757484967d6152013bd6c03f0e102c8e50f7e35.png)

