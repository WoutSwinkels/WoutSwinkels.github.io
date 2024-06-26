---
title: Unreal Engine Learning Path
author: Wout
date: 2024-05-06 18:00:00 +0200
categories: [Unreal Engine, Learning Path]
tags: [UE5, Learning, Game Development, LBVR]
---

## Setup
* CPU: Intel Core i5-13600KF
* GPU: Inno3D GeForce RTX 4070 TWIN X2
* RAM: 64 GB
* OS:  Windows 10 Pro 22H2

## Summary
In September 2023, I installed Unreal Engine 5.3 with the aim of starting to develop small VR games, specifically [Location-Based Virtual Reality (LBVR)](https://www.roadtovr.com/road-testing-location-based-vr-gibbys-guide/) games. This blog post is dedicated to documenting my learning path in Unreal Engine. The most difficult thing for me was to find a good starting point, given Unreal Engine's complexity and the overwhelming amount of content available on platforms like Udemy, YouTube, Epic Games Dev Community, and others. The aim of this blog post is to guide others in their Unreal Engine journey so that they don’t have to figure out how to get started. The plan is to list each tutorial/course that I have followed, together with a short description. Suggestions are always welcome.

## Learning path
### Your First Hour in Unreal Engine 5.2
This tutorial can be found on the [Epic Games Dev Community](https://dev.epicgames.com/community/learning/courses/3ke/your-first-hour-in-unreal-engine-5-2/vvdk/your-first-hour-in-unreal-engine-5-2-overview).

In this tutorial, you will learn to navigate through the GUI of Unreal Engine 5. However, only the basics are covered, including:

1. Creating a new project
2. Navigating the Viewport, Content Browser, and other GUI features
3. Adding assets to your project
4. Migrating assets from an existing project
5. Setting up levels with a Player Spawn, simple meshes, and basic lighting
6. Exploring different types of light and visual settings
7. Introducing very basic Blueprint concepts
8. Packaging your project for sharing or shipping

According to the tutorial, the duration of the video material is 51 minutes, but it took me significantly longer to complete the tutorial. I had to pause the videos quite a bit to be able to follow along, so keep that in mind. One of the most interesting aspects of this tutorial was the reference to the [Content Examples](https://docs.unrealengine.com/5.3/en-US/content-examples-sample-project-for-unreal-engine/) available on the Marketplace. This is a very useful tool for reference when developing.

Unreal Engine: 5.3\\
Completed: September 2023

### Create a First Person Shooter Game in Unreal Engine 5 - Beginners Course
This [course](https://virtushub.com/p/fps) is developed by [Virtus Hub](https://virtushub.com/). You will learn how to develop a first-person shooter in UE5 over the span of 5 days. The tutorials of the first day are free of charge and can also be found on [YouTube](https://www.youtube.com/playlist?list=PLL0cLF8gjBprQbgS7HaBIsjgXQYEeG1zX). After that, you will need to pay $24.99 to unlock the rest of the course. The focus of the course is primarily on gameplay rather than modeling, creating environments, etc. You don’t have to buy the course to view the outline of the rest of the content, which allows you to see if these topics are of interest to you.

Another benefit is that there is a [Discord](https://www.discord.gg/virtushub) server that you can join. There, you can ask questions when you are stuck while following the course. However, the Discord server is not tailored specifically towards the course but to Unreal Engine in general. Therefore, you can also join the server without signing up for the course.

This course is a good starting point even if you have never used Unreal Engine before. Personally, I played the videos on the Virtus Hub website at 1.5x the normal speed. For some reason, I was not able to play the videos in Firefox. Therefore, I used Edge (Yeah, I know, don’t judge me.) This was very doable. Again, you will need to pause a lot and will spend significantly more time than just the duration of the videos, but this will be a constant throughout the tutorials that I have followed.

Unreal Engine: 5.3.2\\
Completed: April 17, 2024

### Unreal Engine 5 VR Blueprint Crash Course
This course is developed by [Artem Chaika](https://www.linkedin.com/in/artem-chaika-5892a9102/) and can be purchased on [Udemy](https://www.udemy.com/course/unreal-engine-5-vr-blueprint-crash-course) for €19.99, although there are often discounts available. Personally, I paid €14.99. Unlike the previous course, this one is specifically tailored towards VR and focuses on developing different features like grabbing, pulling, throwing, etc., in VR and using these features in small game prototypes.

Although the course description mentions that it is suitable for absolute beginners, I found that having some prior experience in Unreal Engine helped me a lot. I think that if I had started with this course, it would have taken me longer to grasp certain concepts. Nevertheless, I found this course very useful, especially because you start with the Unreal Engine VR template and build further on it.

In the beginning, the different components in the VR template are also explained, but to be honest, this went a little too fast for me, especially because there is quite a lot of boilerplate code that is shipped with the VR template. However, I found that this was not a problem for being able to follow the rest of the course.

Additionally, you also receive a [certificate](https://udemy-certificate.s3.amazonaws.com/image/UC-7e633ead-34ff-4dcd-a0c8-2135d965ee59.jpg?v=1714658838000) upon completion. Personally, I don’t think it carries much weight, but it is a nice addition.

Unreal Engine: 5.3.2\\
Completed: May 02, 2024

### Balancing Blueprints and C++
This course can be found on [Epic Games's Dev Community](https://dev.epicgames.com/community/learning/courses/NGA/unreal-engine-balancing-blueprints-and-c/e9xq/unreal-engine-balancing-blueprints-and-c-overview), is free of charge, takes around 30 minutes to read, and is written by the Epic Online Learning team together with the UE Gameplay Systems team. Up until this point, I had only spent time in Blueprints and wanted to know when to use C++ instead of Blueprints. The key takeaways for me are:

- Cater to the skills of your team. Designer/artist-heavy teams will use Blueprints more. Programmer-heavy teams will use C++ more.
- Offload networking, heavy mathematical computations, persistent, consistently running functions, and utilities to C++. Since Blueprint code runs on a virtual machine layer to talk to C++ code and therefore introduces additional overhead, which can add up when you have too many Blueprints that are consistently active.
- Expose the functionality that the designers and artists need from C++ code while keeping the underlying code shielded. The programmer implements the logic (e.g., fire rifle, target hit, etc.) and exposes the variables, functions, events, etc., needed for the designers and artists to interact with this logic (e.g., amount of ammo, magazine size, reload time, etc.).

Unreal Engine: 5.4.1\\
Completed: May 08, 2024

### Unreal Engine C++ Workshop
This workshop can be found on [Epic Games's Dev Community](https://dev.epicgames.com/community/learning/tutorials/5w4b/unreal-engine-c-workshop), is free of charge, takes around 5 hours, and was delivered at Technically Games 2022. The course encompasses 11 videos. The first 3 videos cover foundational C++. If you are a C++ programmer, you can skip these videos. Although I have an elaborate background in C++ programming, I still watched the videos and found them to be a nice refresher. It is stated that this course is also for programmers with no previous C++ experience, but I think this is a bit optimistic. The three videos explaining some C++ concepts, despite being explained very well, are likely to be confusing.

I had hoped that it would be a hands-on workshop, but it is not. By that, I mean that the instructor explains the concepts, writes and uses example code, but as a participant, you are not really coding. Perhaps this was the case if you attended the workshop at Technically Games 2022, but for the online version, it was not the case.

Finally, I personally found that the last three videos went a little over my head. Not due to the concepts, but the actual implementation in C++. I think this is primarily because I don't have a grasp of the Unreal Engine specific classes that exist at the moment.

Unreal Engine: 5.4.1\\
Completed: May 10, 2024