---
title: 'MuJoCo install tips'
date: 2019-12-02
permalink: /posts/rl/mujoco/
excerpt: "MuJoCo installに関するTips"
tags:
  - rl
---

# MuJoCo Install Tips

MuJoCoインストールでたまにこけるのでTipsを随時追加する

## Mac

### ld: library not found for -lglfw.3 (2019/12/2)

GLFW (Graphic Library Flamework)が見つからないというエラー

```bash
$ pip install mujoco-py
Collecting mujoco-py
...
  ld: library not found for -lglfw.3
  collect2: error: ld returned 1 exit status
  error: command '/usr/local/bin/gcc-9' failed with exit status 1
  ----------------------------------------
  ERROR: Failed building wheel for mujoco-py
  Running setup.py clean for mujoco-py
Failed to build mujoco-py
ERROR: Could not build wheels for mujoco-py which use PEP 517 and cannot be installed directly
```

brewでglfwをインストール

```bash
$ brew install glfw
```

インストール先のパスを確認。

```bash
$ find /usr/local -name libglfw*
/usr/local/Cellar/glfw/3.3/lib/libglfw.dylib
/usr/local/Cellar/glfw/3.3/lib/libglfw.3.dylib
/usr/local/Cellar/glfw/3.3/lib/libglfw.3.3.dylib
```

**ライブラリパスを指定して**mujoco-pyをインストール

```bash
$ LIBRARY_PATH=/usr/local/Cellar/glfw/3.3/lib/ pip install mujoco-py
...
Successfully built mujoco-py
Installing collected packages: mujoco-py
Successfully installed mujoco-py-2.0.2.9
```

動作確認：エラーが起きなければOK

```bash
$ python -c "import mujoco_py"
```

###  BlockingIOError (2019/12/7)

ロックを確保しようとするが失敗している模様で、いつまで経ってもプロセスが進まない。

```python
$ python -c 'import mujoco_py'
...
self.trylock()
  File "/Users/ohtake_i/anaconda3/envs/tf2/lib/python3.7/site-packages/fasteners/process_lock.py", line 217, in trylock
    self._trylock(self.lockfile)
  File "/Users/ohtake_i/anaconda3/envs/tf2/lib/python3.7/site-packages/fasteners/process_lock.py", line 250, in _trylock
    fcntl.lockf(lockfile, fcntl.LOCK_EX | fcntl.LOCK_NB)
BlockingIOError: [Errno 35] Resource temporarily unavailable
```

Twitter上で同じ現象に苦しむKSさんを発見。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">import mujoco_py とか環境作ろうとすると出る&quot;PermissonError: [Errno 13] Permission denied: &#39;..../mujoco_py/generated/mujocopy-buildlock&quot;ってエラーマジでどうしたらええのん<br>指定されたディレクトリ見に行ってもそんなlockファイルないし</p>&mdash; KS (@63556poiuytrewq) <a href="https://twitter.com/63556poiuytrewq/status/1201124564232572928?ref_src=twsrc%5Etfw">December 1, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

解決方法は以下。

```bash
$ cd /path/to/mujoco_py
$ pip install -e .
```

