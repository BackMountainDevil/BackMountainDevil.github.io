# gym->gymnasium atari
- date: 2023-10-8

[gym](https://github.com/openai/gym) 自从 2021 年后，不更新维护了，旧版还可以用，新版的转到 [Gymnasium](https://github.com/Farama-Foundation/Gymnasium)。不过由于更新，一些游戏的运行需要额外操作，比如 gymnasium 已经不含 atari 的 rom，需要到[此处](http://www.atarimania.com/rom_collection_archive_atari_2600_roms.html)下载

以 python 3.10 为例，先安装依赖包

```bash
pip install gymnasium[atari]
pip install gymnasium[accept-rom-license]
```

测试代码

```python
import gymnasium as gym
env = gym.make("Alien-v4")
observation, info = env.reset()

for _ in range(1000):
    action = env.action_space.sample()  # agent policy that uses the observation and info
    observation, reward, terminated, truncated, info = env.step(action)

    if terminated or truncated:
        observation, info = env.reset()

env.close()
```

报错：`gymnasium.error.Error: We're Unable to find the game "Alien". Note: Gymnasium no longer distributes ROMs. If you own a license to use the necessary ROMs for research purposes you can download them via `pip install gymnasium[accept-rom-license]`. Otherwise, you should try importing "Alien" via the command `ale-import-roms`. If you believe this is a mistake perhaps your copy of "Alien" is unsupported. To check if this is the case try providing the environment variable `PYTHONWARNINGS=default::ImportWarning:ale_py.roms`. For more information see: https://github.com/mgbellemare/Arcade-Learning-Environment#rom-management`

前面已经装了 gymnasium[accept-rom-license]，这里把前面下好的 Roms.rar 解压到 Roms 文件夹（Roms.rar 文件里有两个文件夹，分别是 ROMS、HC ROMS），运行 `ale-import-roms Roms` 来导入 rom，我这里显示很多红色的 `[NOT SUPPORTED]`，但是又显示 `Imported 103 / 103 ROMs`，不知道导入了哪几个，总之再运行上面的代码就不报错了
