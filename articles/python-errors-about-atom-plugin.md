---
title: "pythonをatomで開発する際のプラグインのエラー" 
emoji: "⏯"
type: "tech" 
topics: ["Python","atom","error"]
published: true
---
#linter-flake8のエラー

Linter] Error running Flake8
Make sure `/Users/user_name/.virtualenvs/env1/bin/flake8
` is installed and on your PATH


flalerを入れ直したら治った

`pip install -e git+https://gitlab.com/pycqa/flake8#egg=flake8
`

