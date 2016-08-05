---
title: system log extract from chromium code
date: 2016-07-15 16:33:10
tags:
categories:
---

glog是Google的一个应用程序的日志库。它提供基于C++风格的流的日志API，以及各种辅助的宏。打印日志只需以流的形式传给 LOG(level) ，例如：
```cpp
VLOG(INFO) << "File Path: " << file_path;
```
