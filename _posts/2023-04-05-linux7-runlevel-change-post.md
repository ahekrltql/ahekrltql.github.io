---
layout: post
title: "CentOS 7 런레벨 변경"
description: "CentOS 7 런레벨(CLI, GUI) 변경"
date: 2023-04-05
tags: [Linux, OS, x86]
comments: true
share: true
---

# 1. CLI 변경

[영구적 변경]

```bash
systemctl set-default multi-user.target
```

[일시적 변경]

```bash
systemctl isolate multi-user.target
```

# 2. GUI 변경

[영구적 변경]

```bash
systemctl set-default graphical.target
```

[일시적 변경]

```bash
systemctl isolate graphical.target
```
