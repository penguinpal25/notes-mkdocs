 # Amazon Polly
- Reverse of Amazon Transcribe
- Turn text into lifelike speech using deep learning
- Allowing you to create applications that talk

## Lexicon & SSML
- Customize the pronunciation of words with Pronunciation Lexicons
	- Stylized words: St3ph4ne => "Stephane"
	- Acronyms: AWS => "Amazon Web Services"
- Upload the lexicons and use them in the SynthesizeSpeech operation
- A lexicon file (.xml, .pls) can have up to 4,000 characters and up to 100 pronunciation rules.
- Generate speech from plain text or from documents marked up with  Speech Synthesis Markup Language (SSML) - enables more customization
	- emphasizing specific words or phrases
	- using phonetic pronunciation 
	- including breathing sounds, whispering
	- using Newscaster speaking style

**Use cases**
- Content creation (audio can be used as complimentary media to written and/or visual communication)
- E-learning
- Telephony (contact centers can engage customers with natural sounding voices)

[Documentation](https://docs.aws.amazon.com/polly/latest/dg/what-is.html)

