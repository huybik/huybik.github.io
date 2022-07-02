---
layout: post
title: Blueeye.com Review - Data Labeling on online Data Platform
---
<p align="center">
<img class="figure" src='https://github.com/huybik/huybik.github.io/blob/main/_posts/blueeye%20data%20platform%20review/image%20label.png?raw=true'>
</p>

### Preface

Company and researcher alike looking to push advancement in deep learning rely heavily on large amount of data. The field is leaning toward self supervise learning ([link](https://ai.facebook.com/blog/self-supervised-learning-the-dark-matter-of-intelligence/)) to lessen the reliance on labeled data, we still needs huge amount of data with human label nevertheless. The task of labeling itself is repetitive and dull, understandably not many people like it except for a few (the Human benchmark Andrej Karpathy is one example [link](http://karpathy.github.io/2014/09/02/what-i-learned-from-competing-against-a-convnet-on-imagenet/)). Labeling large dataset requires significant amount of time, taking away good engineer time, so the task usually ends up in the hands of people from crowd sourcing platform like Amazon Mechanical Turk. A good label tool help to speed up the task, keep label consistent, requires less training and ultimately increases label quality. Many company want to significantly boost labeler efficiency by building the label interface and related workflows from scratch and [blueeye.ai](https://blueeye.ai) is one of such platform. One strength of blueeye.ai is the ability to scale by offering fully cloud based service that requires zero setup on the labeler, while the flow is provided by planner. Let's take a look at what they have to offer.

### A Label Tool With Computer Vision Focus

Blueeye.ai targets computer vision projects which requires vast amount of label data, think of helping autonomous vehicle with only cameras to navigate the street. They offer two mode, image and video labeling.

Image project is setup nice and clear, with team management features like Issues and Project Members. Labeling is mostly painless, with you being offered all the popular shape to mark your target like bounding box, landmark point, polygonal segmentation, polyline, cuboid, etc. The most interesting tool is AI assisted features, which detects all objects in the image and offer initial bounding boxes. The default model being used is YOLO V4, which gives pretty good accuracy and speed. 
<p align="center">
<img class="figure" src='https://github.com/huybik/huybik.github.io/blob/c7708aa53c26bc108a97cbb40d5ffc03ac3a0e42/_posts/blueeye%20data%20platform%20review/image%20labeling.png?raw=true'>
</p>

Modern AI models usually require more refined label like 3d shape and segmentation mask than a simple bounding box, so segmentation is being offered as first class citizen in blueeye.ai, with automated AI assist. I like the way I can drag bounding box and the car is segmented out. The result is not perfect, still it does help reduce a significant amount of time. Brush and eraser is provided to refine the segment. I do prefer to have adjustable joins for a more accurate control.
<p align="center">
<img class="figure" src='https://github.com/huybik/huybik.github.io/blob/c7708aa53c26bc108a97cbb40d5ffc03ac3a0e42/_posts/blueeye%20data%20platform%20review/object%20detection.png?raw=true'>
</p>

#### Video Project
Video is made up by combining multiple images. A simple thought will lead us to believe if we can label multiple image, then the video problem is done. That's is technically correct, but efficiency will be terrible. So the tracking tool is provide to track what has been labeled for multiple frames, by using AI object tracking. Other than that, there are bounding box, cuboid, polygonal ... to label object in a frame. Transcription in the form of subtitle helps labeler get more detail from the scene if needed. 

<video src='https://user-images.githubusercontent.com/5626203/176987678-ecdcbb44-9d5c-45dc-abaa-0576f25fbda7.mp4?raw=true' width=180/>


#### Project Management

When doing labeling, the engineers prefer to do crowd sourcing for resource saving. It turns out management and ease of use becoming more and more an important for high quality label result. blueeye.ai offer good coverage on collaboration flow to get the process go smoothly. Tools like assignee, time tracking, bug tracking and most importantly, it's online based nature makes scaling the team becomes trivial task: many people can join and collaborate on the same project instantly.
<p align="center">
<img class="figure" src='https://github.com/huybik/huybik.github.io/blob/c7708aa53c26bc108a97cbb40d5ffc03ac3a0e42/_posts/blueeye%20data%20platform%20review/project%20management.png?raw=true'>
</p>

Advantages of web based platform is that everything is synced automatically while project management like nature of blueeye.ai can only help to boost productivity and ease of use. However, performance of online platform usually on the low side when compare with similar local apps, like video frame seeking is choppy when transmitting frames online. The AI assistant in video labeling may take a bit of time to run but this is more on server performance.

### Final Thought

The platform strength goes into project management and AI assisted features. At this point in time, object segmentation and tracking are supported for image and video. I only touch image and video project, while AI data span from text, audio, user's interaction and more. While I always say simulation is the future of AI data, where ground truth and context can be inferred precisely and easily. Nvidia' Omni-verse and Facebook Meta's can be seen as simulations that you can interact with, where next gen AI will benefit greatly from. Blueeye is young and already have a strong foundation, I'm excited to see how they expand upon that.
