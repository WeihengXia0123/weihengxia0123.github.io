---
title: "Linux Sed"
date: 2020-11-22T23:54:54+01:00
draft: false
---
Basic linux sed commands to search, replace, and add strings in files.
<!--more-->

## Find lines with certain [string] and replace the whole [line]
sed -i "/[string]/c [new_line]" filePath