---
title: How to Get Ics Files From External Shared Calendars
published: true
description: Get easily ics files from external shared calendars in Google Calendar.
tags: calendar, google, ics
cover_image: https://res.cloudinary.com/dmrgfufa4/image/upload/v1598668440/articles/ics%20from%20google%20calendar/Capture_d_e%CC%81cran_2020-08-28_a%CC%80_21.33.15.png

layout: post
date: 2020-08-28 12:34:51 -0500
categories: calendar google ics
---

There can be some scenarios where you require to have all your shared calendars in a `.ics` file. In my case, it was the need to have the MLH calendar, that the staff shared to us, shared with my agenda in Spacemacs.

For doing this, I had to follow the next steps.

# Get the link

Inside the configuration of the shared calendar, you have to get the **calendar public URL**.

![img](https://res.cloudinary.com/dmrgfufa4/image/upload/v1598665559/articles/ics%20from%20google%20calendar/pic1.png "Image of where to get the calendar public URL")

# Get the Calendar Id

Now you have to get `src` part from the url, for example, if the original url was

    https://calendar.google.com/calendar/embed?src=en-gb.christian%23holiday%40group.v.calendar.google.com&ctz=Europe%2FParis

    en-gb.christian%23holiday%40group.v.calendar.google.com

# Join the Calendar Id with the Calendar feed

The calendar feed usual structure is the following:

    https://calendar.google.com/calendar/ical/**********/public/basic.ics

You need to replace the `*` with the url you got in the step 2. In this case, it will end as the following.

    https://calendar.google.com/calendar/ical/en-gb.christian%23holiday%40group.v.calendar.google.com/public/basic.ics

# Download the ics file

Now that you have the file URL, you can download it using `wget`.

    wget -O my_cal.ics https://calendar.google.com/calendar/ical/en-gb.christian%23holiday%40group.v.calendar.google.com/public/basic.ics

This information was hard to find for me, and if you find it useful, leave a like please. The original answer that provided this solution is found [here](https://support.google.com/calendar/thread/7353749?msgid=17912822) .
