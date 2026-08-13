# 项目包概览

简要说明工作区中主要包的用途，以及它们在仿真与真机中的适用性。

**包说明**

- **champ**: 核心控制框架与系统集成包，包含四足控制器、控制参数、导航配置和示例 URDF。`champ` 提供：
  - 控制器节点、消息桥接与控制接口（C++ 实现）。
  - 配置（`champ_config`）与导航（`champ_navigation`）集成。
 适用性：可用于仿真（结合 `champ_gazebo` 和 URDF）以及真机（使用生成的 robot config 和硬件驱动）。

- **champ_teleop**: 遥控（teleop）节点，基于 `teleop_twist_keyboard` 的分支，扩展以控制机器人整体姿态（roll/pitch/yaw）。
  适用性：可在 RViz 和真实机器人上用于手动控制或调试姿态；常用于演示与调试（RViz teleop）。

- **yocs_velocity_smoother**: 速度平滑器节点（velocity smoother），用于平滑导航命令（`cmd_vel`），此仓库的分支增加了对 y 轴速度平滑的支持。
  适用性：在导航管线中对仿真和真机均有用，能减少速度突变导致的不稳定性。

**哪个用于仿真 / 真机**

- 专门用于仿真的模块与资源：
  - `champ_gazebo`、URDF（`champ_description/urdf`）以及 Gazebo launch 文件（例如 `champ_config` 中的 `gazebo.launch`）是为仿真准备的。要在仿真中运行机器人，请使用：

```bash
roslaunch champ_config gazebo.launch
```

- 通用模块（既可仿真也可真机）：
  - `champ`（控制器）、`champ_teleop`（遥控）、`yocs_velocity_smoother`（速度平滑）都设计为既能在仿真中运行，也能在真实机器人上运行。区别在于真机需要真实的硬件驱动与配置包（由 `champ_setup_assistant` 生成或手工提供）。

**快速使用提示**

- 在不连接真实硬件时，可先用 RViz 验证：

```bash
roslaunch champ_config bringup.launch rviz:=true
roslaunch champ_teleop teleop.launch
```

- 在连接真实机器人时，应使用你生成的 robot config：

```bash
roslaunch <myrobot_config> bringup.launch lite:=true
```

**相关路径**

- [src/champ](src/champ)
- [src/champ_teleop](src/champ_teleop)
- [src/yocs_velocity_smoother](src/yocs_velocity_smoother)

如果你需要，我可以：
- 把这份 README 内容合并到其它文档（例如 `src/CMakeLists.txt` 同目录下的 README），或
- 帮你生成一个用于真机快速上电/仿真启动的操作步骤脚本。
