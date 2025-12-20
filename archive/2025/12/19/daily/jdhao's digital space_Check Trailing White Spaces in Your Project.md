---
title: Check Trailing White Spaces in Your Project
url: https://jdhao.github.io/2025/12/19/check_trailing_white_spaces/
source: jdhao's digital space
date: 2025-12-19
fetch_date: 2025-12-20T03:20:01.330696
---

# Check Trailing White Spaces in Your Project

[↓Skip to main content](#main-content)

[jdhao’s digital space](/)[Home](/)[Archives](/archives/)[Categories](/categories/)[Tags](/tags/)[About](/about/)

* [Home](/)
* [Archives](/archives/)
* [Categories](/categories/)
* [Tags](/tags/)
* [About](/about/)

1. [jdhao's digital space](/)/
2. [Posts](/archives/)/
3. [Check Trailing White Spaces in Your Project](/2025/12/19/check_trailing_white_spaces/)/

# Check Trailing White Spaces in Your Project

19 December 2025·345 words·2 mins·

Note
Shell
Git

Table of Contents

* + [with grep](#with-grep)
  + [with git](#with-git)
  + [references](#references)

Table of Contents

* + [with grep](#with-grep)
  + [with git](#with-git)
  + [references](#references)

When I am working on a Python project, I am using [black](https://github.com/psf/black) to format the code,
so that we have a unified format across the code base.

One pain point, however, is the super annoying trailing white spaces:

1. black can remove trailing spaces for doc string, code and comment, but it will not
   touch trailing spaces in, e.g., multi-line strings.

```
my_multiline_str = """
this is a string that
spans multiple line.
<space><space>
The above line has trailing spaces that black won't touch
"""
```

2. For other files such as YAML, black can not format those
3. Not everyone is using the same tool, which makes enforcement of removing trailing white spaces locally hard

So I want to check if any of the files tracked by git has trailing spaces, and if yes, break the pipeline.
This will force everyone to be aware of the issue and fix them on their side.

How do we do this then?

## with grep [#](#with-grep)

We can achieve this with grep:

```
if grep --recursive --line-number  --exclude-dir="*cache*" '[[:blank:]]$' src/; then
    echo "Trailing whitespaces found"
    exit 1
fi
```

In the above, option `--exclude-dir` is used to exclude some directories to reduce noise.
The `exit 1` is used to break the pipeline run.

Or, you can combine grep with git:

```
if git ls-files | xargs grep --recursive --line-number '[[:blank:]]$'; then
    echo "Trailing whitespaces found"
    exit 1
fi
```

## with git [#](#with-git)

The `git diff --check` command can check if there are trailing white spaces in your current diff,
however, if you have committed the code changes, this command is useless.
This means that in the pipeline, this command does not work since the code is already committed.

Base on [this post](https://peter.eisentraut.org/blog/2014/11/04/checking-whitespace-with-git) here, we can instead use the `git diff-tree` command.

```
git diff-tree --check $(git hash-object -t tree /dev/null) HEAD
```

The command `git hash-object -t tree /dev/null` just compute an empty hash,
so that we can compare the current code against “EMPTY”.
With this trick, we can check the trailing white spaces even if the code is committed.

## references [#](#references)

* find file with trailing white spaces: <https://stackoverflow.com/a/29308492/6064933>

## Related

[Better Git log

25 January 2021·Updated: 27 October 2022·275 words·2 mins

Note
Git](/2021/01/25/better_git_log/)[Liveness and Readiness Check in Kubernetes

20 September 2024·213 words·1 min

Note
Kubernetes
GCP](/2024/09/20/kubernetes_liveness_readiness_check/)[Git line ending config

21 February 2024·450 words·3 mins

Git
Git](/2024/02/21/git_line_ending_config/)

---

[菜谱：茄子肉丁
19 November 2025
→
←](/2025/11/19/braised_eggplant_with_pork/)

---

Please enable JavaScript to view the [comments powered by utterances.](https://github.com/utterance)

[↑](#the-top "Scroll to top")

© 2017 - 2025 ❤️ jdhao

Powered by [Hugo](https://gohugo.io/) & [Blowfish](https://blowfish.page/)