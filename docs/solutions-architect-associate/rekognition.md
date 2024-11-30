# Amazon Rekognition

- Find objects, peoples, texts, scenes in images and videos using ML
- Facial analysis and Facial search to do user verification and people counting
- You can create a database of familiar faces and compare against celebrities

**Use cases:**

- Labelling
- Content moderation
- Text detection
- Face detection and analysis (age, gender, range, emotions...)
- Face search and verification
- Celebrity detection
- Pathing (ex: for sports game analysis)
- Video segment detection
- Streaming video events detection

## Amazon Rekognition - Content Moderation
- Detect content that is inappropriate, unwanted or offensive (ex: nudity, pornography, violence...)
- Used in social media, broadcast media, e-commerce, and advertising situations to create a safer user experience
- Set a Minimum confidence threshold for items that will be flagged

**image -> Amazon Rekognition -> Confidence level & Threshold -> Optional manual review in A2I**
- Flag sensitive content for manual review in Amazon Augmented AI (A2I)
- Help comply with regulations

## Amazon Rekognition - Identity Verification
- Validate selfie picture
- Compare selfie picture with User ID
- Detect duplicate users
- Classify ID document (ex: driver's license, passport...)
- Extract user data
 
![enter image description here](https://d1.awsstatic.com/Amazon-Rekognition_Diagram_Identity-Verification_Use-Case.db65dbca5b05473b42e17b218496ed3a820fd13d.png)
## Amazon Rekognition - Media analysis
- Black frames detection (ex: to insert advertisements)
- Credits detection (detect opening and ending credits to insert binge makers such as next episode...)
- Shot detection (Count start, end, and duration of shots which can be used for promotional videos)
- Color bars detection (detect sections of video that display SMPTE color bars)
- Slates & studio logo detection (detect and remove the slate when preparing content for final reviewing)

## Amazon Rekognition - Send connected home smart alerts

- Accurate detection of people, pets, and packages even in varying lighting conditions
- Integrate Amazon Kinesis Video Streams with Amazon Rekognition Streaming Video Events to enable live video stream analysis.
- Specify video duration (can control how much video you need to process)
- Choose relevant objects (can choose desired objects such as pets, people, and packages for detection from live stream)
- Create real-time alerts (When detects people, pets or packages, it sends a smart alert that includes the video stream output with the detected label, bounding boxes, hero image, and the time-stamp)
- Create bespoke experiences (such as smart search to find specific events of people, pets or packages, smart alert with Alexa for announcements)
 
![enter image description here](https://d1.awsstatic.com/product-marketing/Rekognition/Amazon-Rekognition-Streaming-Video-Events_HIW@2x.1101590fd38815a9ee9400f1c13462db514683fa.png)

## Amazon Rekognition Custom Labels
- How it works

![image](https://user-images.githubusercontent.com/65948438/198953609-e6aa24f3-36fd-4cb4-b32b-233886540b7e.png)

