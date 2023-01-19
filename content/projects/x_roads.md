---
title: "X-roads"
date: 2019-10-11T21:29:52+02:00
draft: false
---
Codes: [GitHub](https://github.com/Tabas32/xroads)

**X-roads** was project that meant to simulate cross road controlled by smart traffic lights. I was part of team of four people and my particular task was to develop AI that recognize robots in our simulation.

Our cross road was just application that we displayed on television. We was able to connect to this cross road via MQTT protocol and change lights on semaphores.
<img src="/static/xroads1.jpg" alt="Cross road" width="600"/>


We have trained Image AI neural network to recognize Ozobots on road. Ozobots are little robots that follow black line.

<img src="/static/xroads2.jpg" alt="Ozobot" width="600"/>

Based on positions that AI found, we decided which traffic light should be green for most fluent traffic.
