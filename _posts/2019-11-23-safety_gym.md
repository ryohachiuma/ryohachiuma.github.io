---
title: 'Safety Gym'
date: 2019-11-23
permalink: /posts/rl/safegym/
excerpt: "SafetyGymが公開されたので試してみた<br/>![](/images/191123_safegym_gif0.gif)![](/images/191123_safegym_gif1.gif)![](/images/191123_safegym_gif2.gif)"
tags:
  - rl
---

# Safety Gym

19/11/21にOpen AIから安全上の制約を満たしながらエージェントの強化学習を行うための環境 [Safety Gymが発表された](https://openai.com/blog/safety-gym/) ので試してみた。

## DDPGで学習させてみる

実験用スクリプト `run_ddpg_safegym.py` を作成。

```python
import safety_gym
import gym

from tf2rl.algos.ddpg import DDPG
from tf2rl.experiments.trainer import Trainer


if __name__ == '__main__':
    parser = Trainer.get_argument()
    parser = DDPG.get_argument(parser)
    parser.set_defaults(batch_size=100)
    parser.set_defaults(n_warmup=10000)
    args = parser.parse_args()

    env = gym.make('Safexp-PointGoal1-v0')
    test_env = gym.make('Safexp-PointGoal1-v0')
    policy = DDPG(
        state_shape=env.observation_space.shape,
        action_dim=env.action_space.high.size,
        gpu=args.gpu,
        memory_capacity=args.memory_capacity,
        max_action=env.action_space.high[0],
        batch_size=args.batch_size,
        n_warmup=args.n_warmup)
    trainer = Trainer(policy, env, args, test_env=test_env)
    if args.evaluate:
        trainer.evaluate_policy(0)
    else:
        trainer()
```

まずは学習。上記コードを `run_ddpg_safegym.py` として保存。

```bash
$ python run_ddpg_safegym.py
```

実行すると学習結果が `results` ディレクトリ以下に生成されているので、それを指定して学習結果を可視化。動画に保存したい場合は `--save-test-progress` の代わりに `--save-test-movie` をつけると同じく `results` 以下にGIFファイルが生成される。

```bash
$ python run_ddpg_safegym.py --evaluate --model-dir /path/to/results --test-episodes 10 --show-test-progress
```

`env.reset()` を呼ぶたびに障害物の位置が変わり、衝突するときにエージェントが赤色で表示されるようになっている。分かりやすくてありがたい。

![](/images/191123_safegym_gif0.gif)![](/images/191123_safegym_gif1.gif)![](/images/191123_safegym_gif2.gif)

## Hack

### 環境の設定を変えたい

`Engine` クラスに辞書型で環境の定義を渡すことで変更可能。便利。例えば下記のように設定する。

```python
from safety_gym.envs.engine import Engine

config = {
    'robot_base': 'xmls/car.xml',
    'task': 'push',
    'hazards_num': 12,
    'vases_num': 4,
}

env = Engine(config)
```

フィールド全体のサイズも変更可能： `placements_extents`

### 障害物の位置を取得 or 変更

そもそもただのMuJoCoモデルなので、[mujoco-py](https://github.com/openai/mujoco-py) の流儀に従う。

```python
import numpy as np
from safety_gym.envs.engine import Engine

env = Engine({'hazards_num': 3})
env.reset()

# Get hazards locations
print(env.hazards_pos)
# [array([-0.932219  , -0.01631583,  0.02      ]), array([1.4262743 , 0.86972011, 0.02      ]), array([-0.80724646,  0.84801085,  0.02      ]), array([ 1.47077378, -1.58439231,  0.02      ])]
[array([0.  , 0.  , 0.02]), array([0.5 , 0.5 , 0.02]), array([1.  , 1.  , 0.02]), array([1.5 , 1.5 , 0.02])]

# Change hazards locations
for i in range(env.hazards_num):
    pos = np.array([i / env.hazards_num * env.placements_extents[2],
                    i / env.hazards_num * env.placements_extents[3],
                    0.02])
    env.model.body_pos[env.model._body_name2id["hazard{}".format(i)]] = pos
env.step(np.zeros_like(env.action_space.shape))

print(env.hazards_pos)
# [array([0.  , 0.  , 0.02]), array([0.5 , 0.5 , 0.02]), array([1.  , 1.  , 0.02]), array([1.5 , 1.5 , 0.02])]
```

### 障害物との接触判定

[safety gym に実装されているコード](https://github.com/openai/safety-gym/blob/master/safety_gym/envs/engine.py#L1140-L1155)を流用する。