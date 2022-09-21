# pamac 安装 ros-noetic-desktop-full(AUR) 失败后如何清理残留的问题
- date: 2022-09-21
- lastmod: 2022-09-21

ros-noetic-desktop-full 安装了将近半个月，网络问题中间某些包就会下载失败，前几天是包都装的差不多了，但是装着装着把我电脑搞黑屏了，重新尝试安装又黑屏了，怕了怕了。于是想着怎么卸载的问题。

ros-noetic-desktop-full 这个包构建没有成功，其四个依赖 ros-noetic-desktop， ros-noetic-simulators， ros-noetic-perception，ros-noetic-urdf-sim-tutorial 也都没有构建成功，于是我在论坛问[About Pamac build ros-noetic-desktop-full(AUR) fail](https://forum.manjaro.org/t/about-pamac-build-ros-noetic-desktop-full-aur-fail/120867)，但是没有有效办法，可能是提问方式不对。然后自己想了一下，pamac有历史记录功能，可以通过历史记录逐个卸载。然后下面是历史记录

## pamac history

<details>
<summary>已经剔除我明确知道是安装/卸载/升级的某些软件的记录，如codium、wps、wemeet、fcitx5</summary>

```bash
[2022-09-20T17:11:07+0800] [ALPM] installed ros-noetic-pcl-msgs (0.3.0-1)
[2022-09-20T17:11:07+0800] [ALPM] installed openni2 (2.2.0-2)
[2022-09-20T17:06:26+0800] [ALPM] installed ros-noetic-urdfdom-py (0.4.6-1)
[2022-09-20T17:06:26+0800] [ALPM] installed ros-noetic-image-pipeline (1.16.0-1)
[2022-09-20T17:05:50+0800] [ALPM] installed ros-noetic-image-publisher (1.16.0-1)
[2022-09-20T17:05:20+0800] [ALPM] installed ros-noetic-depth-image-proc (1.16.0-1)
[2022-09-20T17:05:20+0800] [ALPM] installed ros-noetic-stereo-image-proc (1.16.0-1)
[2022-09-20T17:05:20+0800] [ALPM] installed ros-noetic-camera-info-manager (1.12.0-1)
[2022-09-20T17:03:14+0800] [ALPM] installed ros-noetic-image-proc (1.16.0-2)
[2022-09-20T17:03:14+0800] [ALPM] installed ros-noetic-image-rotate (1.16.0-1)
[2022-09-20T17:03:14+0800] [ALPM] installed ros-noetic-joint-state-controller (0.20.0-1)
[2022-09-20T17:01:35+0800] [ALPM] installed ros-noetic-controller-interface (0.19.5-1)
[2022-09-20T17:01:12+0800] [ALPM] installed ros-noetic-hardware-interface (0.19.5-1)
[2022-09-20T17:01:12+0800] [ALPM] installed ros-noetic-nodelet-tutorial-math (0.2.0-1)
[2022-09-20T17:01:12+0800] [ALPM] installed flann (1.9.1-7)
[2022-09-20T17:01:12+0800] [ALPM] installed ros-noetic-control-toolbox (1.19.0-1)
[2022-09-20T17:01:12+0800] [ALPM] installed ros-noetic-turtle-tf (0.2.3-1)
[2022-09-20T16:56:15+0800] [ALPM] installed ros-noetic-self-test (1.11.0-1)
[2022-09-20T16:56:15+0800] [ALPM] installed ros-noetic-control-msgs (1.5.2-1)
[2022-09-20T16:56:15+0800] [ALPM] installed ros-noetic-ros-tutorials (1.3.1-1)
[2022-09-20T16:54:57+0800] [ALPM] installed ros-noetic-rospy-tutorials (0.10.2-1)
[2022-09-20T16:54:57+0800] [ALPM] installed ros-noetic-camera-calibration (1.16.0-1)
[2022-09-20T16:54:57+0800] [ALPM] installed ros-noetic-roscpp-tutorials (0.10.2-1)
[2022-09-20T16:53:48+0800] [ALPM] installed ros-noetic-image-geometry (1.16.0-1)
[2022-09-20T16:07:09+0800] [ALPM] installed ignition-transport-8 (8.2.0-2)
[2022-09-20T16:07:09+0800] [ALPM] installed ignition-fuel_tools-4 (4.5.0-1)
[2022-09-20T16:05:01+0800] [ALPM] installed sdformat-9 (9.8.0-1)
[2022-09-20T16:05:01+0800] [ALPM] installed ignition-msgs-5 (5.9.1-2)
[2022-09-20T15:57:45+0800] [ALPM] installed ignition-common-3 (3.14.1-1)
[2022-09-20T15:57:45+0800] [ALPM] installed libccd (2.1-1)
[2022-09-20T15:57:45+0800] [ALPM] installed ignition-tools (1.5.0-1)
[2022-09-20T15:55:36+0800] [ALPM] installed ignition-math (6.13.0-1)
[2022-09-20T15:53:11+0800] [ALPM] installed ros-noetic-viz (1.5.0-1)
[2022-09-20T15:53:11+0800] [ALPM] installed ros-noetic-joint-state-publisher (1.15.1-1)
[2022-09-20T15:53:11+0800] [ALPM] installed ros-noetic-rviz-python-tutorial (0.11.0-1)
[2022-09-20T15:53:11+0800] [ALPM] installed ros-noetic-visualization-marker-tutorials (0.11.0-1)
[2022-09-20T15:53:11+0800] [ALPM] installed ignition-cmake (2.15.0-1)
[2022-09-20T15:53:11+0800] [ALPM] installed ros-noetic-turtlesim (0.10.2-1)
[2022-09-20T15:50:24+0800] [ALPM] installed ros-noetic-rqt-common-plugins (0.4.9-1)
[2022-09-20T15:50:07+0800] [ALPM] installed ros-noetic-rqt-reconfigure (0.5.5-1)
[2022-09-20T15:49:40+0800] [ALPM] installed ros-noetic-rqt-srv (0.4.9-1)
[2022-09-20T15:49:40+0800] [ALPM] installed ros-noetic-rqt-py-console (0.4.10-1)
[2022-09-20T15:49:40+0800] [ALPM] installed ros-noetic-rqt-web (0.4.10-1)
[2022-09-20T15:49:40+0800] [ALPM] installed ros-noetic-roslint (0.12.0-1)
[2022-09-20T15:49:40+0800] [ALPM] installed ros-noetic-rqt-publisher (0.4.10-1)
[2022-09-20T15:49:40+0800] [ALPM] installed ros-noetic-rqt-dep (0.4.12-1)
[2022-09-20T15:47:17+0800] [ALPM] installed ros-noetic-webkit-dependency (1.1.2-1)
[2022-09-20T15:47:17+0800] [ALPM] installed ros-noetic-rqt-launch (0.4.9-1)
[2022-09-20T15:47:17+0800] [ALPM] installed ros-noetic-rqt-service-caller (0.4.10-1)
[2022-09-20T15:47:17+0800] [ALPM] installed ros-noetic-rqt-shell (0.4.11-1)
[2022-09-20T15:47:17+0800] [ALPM] installed ros-noetic-rqt-bag-plugins (0.5.1-1)
[2022-09-20T15:45:25+0800] [ALPM] installed ros-noetic-rqt-plot (0.4.13-1)
[2022-09-20T15:45:01+0800] [ALPM] installed ros-noetic-ros-base (1.5.0-1)
[2022-09-20T15:45:01+0800] [ALPM] installed ros-noetic-qwt-dependency (1.1.1-1)
[2022-09-20T15:44:24+0800] [ALPM] installed ros-noetic-ros-core (1.5.0-1)
[2022-09-20T15:43:50+0800] [ALPM] installed ros-noetic-ros-comm (1.15.14-1)
[2022-09-20T15:43:50+0800] [ALPM] installed ros-noetic-common-msgs (1.13.1-1)
[2022-09-20T15:42:49+0800] [ALPM] installed ros-noetic-roslisp (1.9.24-1)
[2022-09-20T15:42:49+0800] [ALPM] installed ros-noetic-shape-msgs (1.13.1-1)
[2022-09-20T15:42:49+0800] [ALPM] installed ros-noetic-ros (1.15.8-1)
[2022-09-20T15:42:49+0800] [ALPM] installed ros-noetic-trajectory-msgs (1.13.1-1)
[2022-09-20T15:41:00+0800] [ALPM] installed ros-noetic-roscreate (1.15.8-1)
[2022-09-20T15:41:00+0800] [ALPM] installed ros-noetic-rosmake (1.15.8-1)
[2022-09-20T15:41:00+0800] [ALPM] installed ros-noetic-rosboost-cfg (1.15.8-1)
[2022-09-20T15:41:00+0800] [ALPM] installed ros-noetic-nodelet-core (1.10.2-1)
[2022-09-20T15:41:00+0800] [ALPM] installed ros-noetic-rosbash (1.15.8-1)
[2022-09-20T15:41:00+0800] [ALPM] installed ros-noetic-rosbag-migration-rule (1.0.1-1)
[2022-09-20T15:41:00+0800] [ALPM] installed ros-noetic-roscpp-core (0.7.2-1)
[2022-09-20T15:41:00+0800] [ALPM] installed ros-noetic-mk (1.15.8-1)
[2022-09-20T15:37:21+0800] [ALPM] installed ros-noetic-bond-core (1.8.6-1)
[2022-09-20T15:37:21+0800] [ALPM] installed ros-noetic-rqt-robot-plugins (0.5.8-1)
[2022-09-20T15:37:21+0800] [ALPM] installed ros-noetic-nodelet-topic-tools (1.10.2-1)
[2022-09-20T15:35:53+0800] [ALPM] installed ros-noetic-rqt-rviz (0.7.0-1)
[2022-09-20T15:35:12+0800] [ALPM] installed ros-noetic-rviz (1.14.14-3)
[2022-09-20T15:29:55+0800] [ALPM] installed ros-noetic-media-export (0.3.0-1)
[2022-09-20T15:29:55+0800] [ALPM] installed ogre-1.9 (1.9.1-7)
[2022-09-20T15:29:55+0800] [ALPM] installed ros-noetic-urdf (1.13.2-5)
[2022-09-20T15:20:18+0800] [ALPM] installed ros-noetic-map-msgs (1.14.1-1)
[2022-09-20T15:20:18+0800] [ALPM] installed ros-noetic-urdf-parser-plugin (1.13.2-1)
[2022-09-20T15:20:18+0800] [ALPM] installed ros-noetic-resource-retriever (1.12.7-1)
[2022-09-20T15:20:18+0800] [ALPM] installed ros-noetic-laser-geometry (1.6.7-4)
[2022-09-20T15:20:18+0800] [ALPM] installed ros-noetic-rosconsole-bridge (0.5.4-2)
[2022-09-20T15:20:18+0800] [ALPM] installed ros-noetic-tf2-eigen (0.7.5-1)
[2022-09-20T15:20:18+0800] [ALPM] installed ros-noetic-rqt-robot-dashboard (0.5.8-1)
[2022-09-20T15:20:18+0800] [ALPM] installed urdfdom (3.1.0-1)
[2022-09-20T15:16:22+0800] [ALPM] installed ros-noetic-rqt-robot-monitor (0.5.14-1)
[2022-09-20T15:16:00+0800] [ALPM] installed ros-noetic-rqt-bag (0.5.1-1)
[2022-09-20T15:16:00+0800] [ALPM] installed ros-noetic-rqt-moveit (0.5.10-1)
[2022-09-20T15:16:00+0800] [ALPM] installed ros-noetic-qt-gui-py-common (0.4.2-1)
[2022-09-20T15:14:38+0800] [ALPM] installed ros-noetic-rqt-robot-steering (0.5.12-1)
[2022-09-20T15:14:38+0800] [ALPM] installed ros-noetic-rqt-pose-view (0.5.11-1)
[2022-09-20T15:14:38+0800] [ALPM] installed ros-noetic-rqt-topic (0.4.13-1)
[2022-09-20T15:13:28+0800] [ALPM] installed ros-noetic-rqt-runtime-monitor (0.5.9-1)
[2022-09-20T15:13:28+0800] [ALPM] installed ros-noetic-rqt-nav-view (0.5.7-2)
[2022-09-20T15:13:28+0800] [ALPM] installed ros-noetic-gl-dependency (1.1.2-1)
[2022-09-20T15:13:28+0800] [ALPM] installed ros-noetic-rqt-tf-tree (0.6.3-1)
[2022-09-20T15:11:51+0800] [ALPM] installed ros-noetic-rqt-graph (0.4.14-1)
[2022-09-20T15:11:26+0800] [ALPM] installed ros-noetic-interactive-markers (1.12.0-2)
[2022-09-20T15:11:26+0800] [ALPM] installed ros-noetic-qt-dotgraph (0.4.2-1)
[2022-09-20T15:10:16+0800] [ALPM] installed ros-noetic-stage-ros (1.8.0-4)
[2022-09-20T15:10:16+0800] [ALPM] installed ros-noetic-visualization-msgs (1.13.1-1)
[2022-09-20T15:10:16+0800] [ALPM] installed ros-noetic-tf2-geometry-msgs (0.7.5-1)
[2022-09-20T15:08:35+0800] [ALPM] installed ros-noetic-stage (4.3.0-4)
[2022-09-20T15:08:35+0800] [ALPM] installed ros-noetic-rqt-image-view (0.4.16-3)
[2022-09-20T15:08:35+0800] [ALPM] installed ros-noetic-nav-msgs (1.13.1-1)
[2022-09-20T15:08:35+0800] [ALPM] installed urdfdom-headers (1.1.0-1)
[2022-09-20T15:08:35+0800] [ALPM] installed ros-noetic-smach (2.5.0-1)
[2022-09-20T15:08:34+0800] [ALPM] installed ros-noetic-geometry (1.13.2-1)
[2022-09-20T15:05:37+0800] [ALPM] installed ros-noetic-tf-conversions (1.13.2-1)
[2022-09-20T15:05:37+0800] [ALPM] installed ros-noetic-eigen-conversions (1.13.2-1)
[2022-09-20T15:04:44+0800] [ALPM] installed ros-noetic-tf (1.13.2-2)
[2022-09-20T15:04:44+0800] [ALPM] installed orocos-kdl-python (1.5.1-1)
[2022-09-20T15:02:56+0800] [ALPM] installed ros-noetic-tf2-ros (0.7.5-1)
[2022-09-20T15:01:58+0800] [ALPM] installed ros-noetic-tf2-py (0.7.5-1)
[2022-09-20T15:01:32+0800] [ALPM] installed ros-noetic-tf2 (0.7.5-1)
[2022-09-20T15:00:51+0800] [ALPM] installed ros-noetic-kdl-conversions (1.13.2-1)
[2022-09-20T15:00:51+0800] [ALPM] installed ros-noetic-tf2-msgs (0.7.5-1)
[2022-09-20T15:00:51+0800] [ALPM] installed ros-noetic-roswtf (1.15.14-1)
[2022-09-20T14:59:56+0800] [ALPM] installed orocos-kdl (1.5.1-1)
[2022-09-20T14:59:56+0800] [ALPM] installed ros-noetic-angles (1.9.13-3)

[2022-09-19T19:10:04+0800] [ALPM] installed ros-noetic-rqt-gui-cpp (0.5.3-1)
[2022-09-19T19:09:32+0800] [ALPM] installed ros-noetic-qt-gui-cpp (0.4.2-2)
[2022-09-19T19:08:41+0800] [ALPM] installed ros-noetic-diagnostic-updater (1.11.0-1)
[2022-09-19T19:08:41+0800] [ALPM] installed ros-noetic-rqt-top (0.4.10-1)
[2022-09-19T19:08:41+0800] [ALPM] installed python-sip4 (4.19.25-3)
[2022-09-19T19:08:41+0800] [ALPM] installed sip4 (4.19.25-3)
[2022-09-19T19:07:29+0800] [ALPM] installed ros-noetic-rqt-action (0.4.9-2)
[2022-09-19T19:07:29+0800] [ALPM] installed ros-noetic-diagnostic-msgs (1.13.1-1)
[2022-09-19T19:07:01+0800] [ALPM] installed ros-noetic-rqt-msg (0.4.10-1)
[2022-09-19T18:55:00+0800] [ALPM] installed ros-noetic-rqt-console (0.4.11-1)
[2022-09-19T18:54:46+0800] [ALPM] installed ros-noetic-rqt-logger-level (0.4.11-1)
[2022-09-19T18:43:54+0800] [ALPM] installed ros-noetic-rqt-gui-py (0.5.3-1)
[2022-09-19T18:43:54+0800] [ALPM] installed ros-noetic-rosnode (1.15.14-1)
[2022-09-19T17:44:42+0800] [ALPM] installed ros-noetic-rqt-py-common (0.5.3-1)
[2022-09-19T17:44:42+0800] [ALPM] installed ros-noetic-rqt-gui (0.5.3-1)
[2022-09-19T17:44:10+0800] [ALPM] installed ros-noetic-qt-gui (0.4.2-1)
[2022-09-19T17:43:54+0800] [ALPM] installed tango-icon-theme (0.8.90-14)
[2022-09-19T17:43:54+0800] [ALPM] installed ros-noetic-python-qt-binding (0.4.4-2)
[2022-09-19T17:41:20+0800] [ALPM] installed python-pyqt5-sip4 (5.15.6-2)
[2022-09-19T17:41:20+0800] [ALPM] installed ros-noetic-bondpy (1.8.6-1)
[2022-09-19T17:41:20+0800] [ALPM] installed ros-noetic-realtime-tools (1.16.1-1)
[2022-09-19T17:41:20+0800] [ALPM] removed python-pyqt5 (5.15.7-1)
[2022-09-19T17:22:09+0800] [ALPM] installed ros-noetic-actionlib (1.13.2-1)
[2022-09-19T17:16:10+0800] [ALPM] installed ros-noetic-image-view (1.16.0-2)
[2022-09-19T17:16:10+0800] [ALPM] installed ros-noetic-compressed-image-transport (1.14.0-1)
[2022-09-19T17:16:10+0800] [ALPM] installed ros-noetic-rostopic (1.15.14-1)
[2022-09-19T17:16:10+0800] [ALPM] installed ros-noetic-actionlib-msgs (1.13.1-1)
[2022-09-19T17:11:37+0800] [ALPM] installed ros-noetic-image-transport (1.12.0-1)
[2022-09-19T17:11:37+0800] [ALPM] installed ros-noetic-stereo-msgs (1.13.1-1)
[2022-09-19T17:10:40+0800] [ALPM] installed ros-noetic-message-filters (1.15.14-1)
[2022-09-19T17:10:40+0800] [ALPM] installed ros-noetic-camera-calibration-parsers (1.12.0-1)
[2022-09-19T17:10:40+0800] [ALPM] installed ros-noetic-dynamic-reconfigure (1.7.3-1)
[2022-09-19T17:10:40+0800] [ALPM] installed ros-noetic-cv-bridge (1.16.0-1)
[2022-09-19T16:30:10+0800] [ALPM] installed ros-noetic-rosservice (1.15.14-1)
[2022-09-19T16:29:55+0800] [ALPM] installed ros-noetic-rosmsg (1.15.14-1)
[2022-09-19T16:29:39+0800] [ALPM] installed ros-noetic-rosbag (1.15.14-1)
[2022-09-19T16:28:48+0800] [ALPM] installed ros-noetic-rosbag-storage (1.15.14-1)
[2022-09-19T16:28:48+0800] [ALPM] installed ros-noetic-topic-tools (1.15.14-1)
[2022-09-19T16:22:56+0800] [ALPM] installed ros-noetic-rostest (1.15.14-1)
[2022-09-19T16:22:30+0800] [ALPM] installed ros-noetic-roslaunch (1.15.14-1)
[2022-09-19T16:22:15+0800] [ALPM] installed ros-noetic-nodelet (1.10.2-1)
[2022-09-19T16:22:15+0800] [ALPM] installed ros-noetic-rosmaster (1.15.14-1)
[2022-09-19T16:22:15+0800] [ALPM] installed ros-noetic-rosparam (1.15.14-1)
[2022-09-19T16:22:15+0800] [ALPM] installed ros-noetic-rosout (1.15.14-1)
[2022-09-19T16:22:15+0800] [ALPM] installed ros-noetic-rosclean (1.15.8-1)
[2022-09-19T16:09:15+0800] [ALPM] installed ros-noetic-pluginlib (1.13.0-1)
[2022-09-19T16:08:55+0800] [ALPM] installed ros-noetic-rospy (1.15.14-1)
[2022-09-19T16:08:55+0800] [ALPM] installed ros-noetic-class-loader (0.5.0-2)
[2022-09-19T16:05:32+0800] [ALPM] installed ros-noetic-rosgraph (1.15.14-1)
[2022-09-19T16:05:32+0800] [ALPM] installed ros-noetic-bondcpp (1.8.6-1)
[2022-09-19T15:42:22+0800] [ALPM] installed ros-noetic-bond (1.8.6-1)
[2022-09-19T15:42:22+0800] [ALPM] installed ros-noetic-smclib (1.8.6-2)
[2022-09-19T15:42:22+0800] [ALPM] installed ros-noetic-roscpp (1.15.14-2)
[2022-09-19T15:29:31+0800] [ALPM] installed ros-noetic-roslang (1.15.8-1)
[2022-09-19T15:29:31+0800] [ALPM] installed ros-noetic-rosconsole (1.14.3-8)
[2022-09-19T15:20:02+0800] [ALPM] installed ros-noetic-rosunit (1.15.8-1)
[2022-09-19T15:19:49+0800] [ALPM] installed ros-noetic-roslib (1.15.8-1)
[2022-09-19T15:19:24+0800] [ALPM] installed ros-noetic-rospack (2.6.2-1)
[2022-09-19T15:10:42+0800] [ALPM] installed python-rosdep (0.22.1-1)
[2022-09-19T15:10:30+0800] [ALPM] installed python-rosdistro (0.9.0-1)
[2022-09-19T15:07:05+0800] [ALPM] installed python-rospkg (1.4.0-1)
[2022-09-19T15:07:05+0800] [ALPM] installed ros-noetic-rosbuild (1.15.8-1)
[2022-09-19T15:07:05+0800] [ALPM] installed ros-noetic-ros-environment (1.3.2-1)
[2022-09-19T15:07:05+0800] [ALPM] installed ros-noetic-rosgraph-msgs (1.11.3-1)
[2022-09-19T14:48:56+0800] [ALPM] installed ros-noetic-sensor-msgs (1.13.1-1)
[2022-09-19T14:48:37+0800] [ALPM] installed ros-noetic-geometry-msgs (1.13.1-1)
[2022-09-19T14:48:20+0800] [ALPM] installed ros-noetic-std-msgs (0.5.13-1)
[2022-09-19T14:40:55+0800] [ALPM] installed ros-noetic-xmlrpcpp (1.15.14-1)
[2022-09-19T14:40:55+0800] [ALPM] installed ros-noetic-roslz4 (1.15.14-1)
[2022-09-19T14:40:55+0800] [ALPM] installed ros-noetic-cmake-modules (0.5.0-1)
[2022-09-19T14:40:55+0800] [ALPM] installed ros-noetic-std-srvs (1.11.3-1)
[2022-09-19T14:39:47+0800] [ALPM] installed ros-noetic-message-generation (0.4.1-2)
[2022-09-19T14:39:28+0800] [ALPM] installed ros-noetic-geneus (3.0.0-1)
[2022-09-19T14:39:28+0800] [ALPM] installed ros-noetic-gencpp (0.6.5-1)
[2022-09-19T14:39:28+0800] [ALPM] installed ros-noetic-gennodejs (2.0.1-1)
[2022-09-19T14:39:28+0800] [ALPM] installed ros-noetic-message-runtime (0.4.13-1)
[2022-09-19T14:39:28+0800] [ALPM] installed ros-noetic-genlisp (0.4.18-2)
[2022-09-19T14:31:08+0800] [ALPM] installed ros-noetic-roscpp-serialization (0.7.2-1)
[2022-09-19T14:30:47+0800] [ALPM] installed ros-noetic-roscpp-traits (0.7.2-1)
[2022-09-19T14:23:43+0800] [ALPM] installed ros-noetic-rostime (0.7.2-1)
[2022-09-19T14:23:16+0800] [ALPM] installed ros-noetic-cpp-common (0.7.2-1)
[2022-09-19T14:22:51+0800] [ALPM] installed console-bridge (1.0.2-1)
[2022-09-19T14:22:51+0800] [ALPM] installed ros-noetic-genpy (0.6.15-1)
[2022-09-19T14:18:43+0800] [ALPM] installed ros-noetic-genmsg (0.5.16-2)
[2022-09-19T14:18:29+0800] [ALPM] installed ros-noetic-catkin (0.8.10-2)
[2022-09-19T14:18:14+0800] [ALPM] installed python-catkin_pkg (0.5.2-1)
[2022-09-19T14:18:14+0800] [ALPM] installed ros-build-tools (0.3.2-2)
[2022-09-19T13:28:11+0800] [ALPM] installed utf8cpp (3.2.1-1)
[2022-09-19T13:28:11+0800] [ALPM] installed opencv (4.6.0-4)
[2022-09-19T13:28:11+0800] [ALPM] installed python-matplotlib (3.5.2-1)
[2022-09-19T13:28:11+0800] [ALPM] installed cppcheck (2.9-1)
[2022-09-19T13:28:11+0800] [ALPM] installed python-pycryptodomex (3.12.0-1)
[2022-09-19T13:28:11+0800] [ALPM] installed gtest (1.12.1-1)
[2022-09-19T13:28:11+0800] [ALPM] installed python-pyaml (21.10.1-1)
[2022-09-19T13:28:11+0800] [ALPM] installed python-empy (3.3.4-7)
[2022-09-19T13:28:11+0800] [ALPM] installed adios2 (2.8.3-1)
[2022-09-19T13:28:11+0800] [ALPM] installed zfp (0.5.5-5)
[2022-09-19T13:28:11+0800] [ALPM] installed pybind11 (2.10.0-1)
[2022-09-19T13:28:11+0800] [ALPM] installed netcdf (4.9.0-3)
[2022-09-19T13:28:11+0800] [ALPM] installed nlohmann-json (3.11.2-1)
[2022-09-19T13:28:11+0800] [ALPM] installed tinyxml (2.6.2-9)
[2022-09-19T13:28:11+0800] [ALPM] installed qt5-xmlpatterns (5.15.5+kde+r0-1)
[2022-09-19T13:28:11+0800] [ALPM] installed nvidia-cg-toolkit (3.1-6)
[2022-09-19T13:28:10+0800] [ALPM] installed google-glog (0.6.0-1)
[2022-09-19T13:28:10+0800] [ALPM] installed doxygen (1.9.3-1)
[2022-09-19T13:28:10+0800] [ALPM] installed qt5-websockets (5.15.5+kde+r3-1)
[2022-09-19T13:28:10+0800] [ALPM] installed gflags (2.2.2-4)
[2022-09-19T13:28:10+0800] [ALPM] installed libfabric (1.15.1-1)
[2022-09-19T13:28:10+0800] [ALPM] installed python-gnupg (0.5.0-1)
[2022-09-19T13:28:10+0800] [ALPM] installed liblas (1.8.1.r128+gded46373-1)
[2022-09-19T13:28:10+0800] [ALPM] installed gdal (3.5.1-3)
[2022-09-19T13:28:10+0800] [ALPM] installed libdeflate (1.11-1)
[2022-09-19T13:28:10+0800] [ALPM] installed intltool (0.51.0-6)
[2022-09-19T13:28:10+0800] [ALPM] installed sbcl (2.2.5-1)
[2022-09-19T13:28:10+0800] [ALPM] installed cgns (4.3.0-3)
[2022-09-19T13:28:10+0800] [ALPM] installed hdf5 (1.12.2-1)
[2022-09-19T13:28:10+0800] [ALPM] installed libaec (1.0.6-1)
[2022-09-19T13:28:10+0800] [ALPM] installed python-mpi4py (3.1.3-2)
[2022-09-19T13:28:10+0800] [ALPM] installed qt5-remoteobjects (5.15.5+kde+r0-1)
[2022-09-19T13:28:10+0800] [ALPM] installed python-numpy (1.23.2-1)
[2022-09-19T13:28:10+0800] [ALPM] installed cblas (3.10.1-1)
[2022-09-19T13:28:10+0800] [ALPM] installed python-cycler (0.11.0-1)
[2022-09-19T13:28:10+0800] [ALPM] installed gtk2 (2.24.33-2)
[2022-09-19T13:28:10+0800] [ALPM] installed qt5-connectivity (5.15.5+kde+r5-1)
[2022-09-19T13:28:10+0800] [ALPM] installed python-kiwisolver (1.4.4-1)
[2022-09-19T13:28:10+0800] [ALPM] installed texlive-core (2022.63035-1)
[2022-09-19T13:28:08+0800] [ALPM] installed texlive-bin (2022.62885-1)
[2022-09-19T13:28:07+0800] [ALPM] installed libsynctex (2022.62885-1)
[2022-09-19T13:28:07+0800] [ALPM] installed potrace (1.16-2)
[2022-09-19T13:28:07+0800] [ALPM] installed harfbuzz-icu (5.1.0-1)
[2022-09-19T13:28:07+0800] [ALPM] installed zziplib (0.13.72-1)
[2022-09-19T13:28:07+0800] [ALPM] installed libsigsegv (2.14-1)
[2022-09-19T13:28:07+0800] [ALPM] installed t1lib (5.1.2-8)
[2022-09-19T13:28:07+0800] [ALPM] installed qt5-serialport (5.15.5+kde+r0-1)
[2022-09-19T13:28:07+0800] [ALPM] installed python-paramiko (2.11.0-1)
[2022-09-19T13:28:07+0800] [ALPM] installed python-pynacl (1.4.0-5)
[2022-09-19T13:28:07+0800] [ALPM] installed tinyxml2 (9.0.0-1)
[2022-09-19T13:28:07+0800] [ALPM] installed sz (2.1.12-4)
[2022-09-19T13:28:07+0800] [ALPM] installed fltk (1.3.8-1)
[2022-09-19T13:28:07+0800] [ALPM] installed python-lxml (4.9.1-1)
[2022-09-19T13:28:07+0800] [ALPM] installed eigen (3.4.0-1)
[2022-09-19T13:28:07+0800] [ALPM] installed python-defusedxml (0.7.1-4)
[2022-09-19T13:28:07+0800] [ALPM] installed numactl (2.0.14-3)
[2022-09-19T13:28:07+0800] [ALPM] installed boost (1.79.0-1)
[2022-09-19T13:28:06+0800] [ALPM] installed pyqt-builder (1.13.0-1)
[2022-09-19T13:28:06+0800] [ALPM] installed icon-naming-utils (0.8.90-5)
[2022-09-19T13:28:06+0800] [ALPM] installed perl-xml-simple (2.25-7)
[2022-09-19T13:28:06+0800] [ALPM] installed perl-xml-sax-expat (0.51-7)
[2022-09-19T13:28:06+0800] [ALPM] installed poco (1.12.1-1)
[2022-09-19T13:28:06+0800] [ALPM] installed unixodbc (2.3.11-1)
[2022-09-19T13:28:06+0800] [ALPM] installed mariadb-libs (10.9.2-1)
[2022-09-19T13:28:06+0800] [ALPM] installed assimp (5.2.4-2)
[2022-09-19T13:28:06+0800] [ALPM] installed libharu (2.4.0-1)
[2022-09-19T13:28:06+0800] [ALPM] installed apr-util (1.6.1-9)
[2022-09-19T13:28:06+0800] [ALPM] installed apr (1.7.0-3)
[2022-09-19T13:28:06+0800] [ALPM] installed python-pydot (1.4.2-3)
[2022-09-19T13:28:06+0800] [ALPM] installed graphviz (5.0.1-1)
[2022-09-19T13:28:06+0800] [ALPM] installed gts (0.7.6.121130-2)
[2022-09-19T13:28:06+0800] [ALPM] installed netpbm (10.73.37-2)
[2022-09-19T13:28:06+0800] [ALPM] installed ffcall (2.4-2)
[2022-09-19T13:28:06+0800] [ALPM] installed openvr (1.16.8-2)
[2022-09-19T13:28:06+0800] [ALPM] installed python-wxpython (1:4.1.1-3)
[2022-09-19T13:28:06+0800] [ALPM] installed wxwidgets-gtk3 (3.2.0-6)
[2022-09-19T13:28:06+0800] [ALPM] installed libmspack (1:0.10.1alpha-3)
[2022-09-19T13:28:06+0800] [ALPM] installed wxwidgets-common (3.2.0-6)
[2022-09-19T13:28:06+0800] [ALPM] installed sip (6.6.2-3)
[2022-09-19T13:28:06+0800] [ALPM] installed python-toml (0.10.2-7)
[2022-09-19T13:28:06+0800] [ALPM] installed swig (4.0.2-5)
[2022-09-19T13:28:06+0800] [ALPM] installed python-fonttools (4.37.1-1)
[2022-09-19T13:28:06+0800] [ALPM] installed ospray (2.10.0-4)
[2022-09-19T13:28:05+0800] [ALPM] installed openvkl (1.3.0-2)
[2022-09-19T13:28:05+0800] [ALPM] installed openvdb (9.1.0-1)
[2022-09-19T13:28:05+0800] [ALPM] installed log4cplus (2.0.8-1)
[2022-09-19T13:28:05+0800] [ALPM] installed blosc (1.21.1-2)
[2022-09-19T13:28:05+0800] [ALPM] installed jemalloc (1:5.3.0-1)
[2022-09-19T13:28:05+0800] [ALPM] installed embree (3.13.4-1)
[2022-09-19T13:28:05+0800] [ALPM] installed qhull (2020.2-4)
[2022-09-19T13:28:05+0800] [ALPM] installed qwt (6.2.0-1)
[2022-09-19T13:28:05+0800] [ALPM] installed libgeotiff (1.7.1-1)
[2022-09-19T13:28:05+0800] [ALPM] installed rkcommon (1.10.0-1)
[2022-09-19T13:28:05+0800] [ALPM] installed glfw-wayland (3.3.8-1)
[2022-09-19T13:28:05+0800] [ALPM] installed tk (8.6.12-1)
[2022-09-19T13:28:05+0800] [ALPM] installed yaml-cpp (0.7.0-2)
[2022-09-19T13:28:05+0800] [ALPM] installed xerces-c (3.2.3-6)
[2022-09-19T13:28:05+0800] [ALPM] installed crypto++ (8.7.0-1)
[2022-09-19T13:28:05+0800] [ALPM] installed qt5-quick3d (5.15.5+kde+r1-1)
[2022-09-19T13:28:05+0800] [ALPM] installed openimagedenoise (1.4.3-1)
[2022-09-19T13:28:04+0800] [ALPM] installed python-dateutil (2.8.2-4)
[2022-09-19T13:28:04+0800] [ALPM] installed hddtemp (0.3.beta15.53-3)
[2022-09-19T13:28:04+0800] [ALPM] installed gl2ps (1.4.2-2)
[2022-09-19T13:28:04+0800] [ALPM] installed vtk (9.1.0-20)
[2022-09-19T13:28:04+0800] [ALPM] installed tbb (2021.5.0-2)
[2022-09-19T13:28:04+0800] [ALPM] installed pugixml (1.12.1-1)
[2022-09-19T13:28:04+0800] [ALPM] installed freeimage (3.18.0-15)
[2022-09-19T13:28:04+0800] [ALPM] installed jxrlib (0.2.4-1)
[2022-09-19T13:28:04+0800] [ALPM] installed python-netifaces (0.11.0-3)
[2022-09-19T13:28:04+0800] [ALPM] installed python-opengl (3.1.6-1)
[2022-09-19T13:28:03+0800] [ALPM] installed jdk-openjdk (18.0.2.1.u0-1)
[2022-09-19T13:28:03+0800] [ALPM] installed java-environment-common (3-3)
[2022-09-19T13:28:03+0800] [ALPM] installed jre-openjdk (18.0.2.1.u0-1)
[2022-09-19T13:28:03+0800] [ALPM] installed jre-openjdk-headless (18.0.2.1.u0-1)
[2022-09-19T13:28:03+0800] [ALPM] installed libnet (1:1.1.6-1)
[2022-09-19T13:28:03+0800] [ALPM] installed java-runtime-common (3-3)
[2022-09-19T13:28:03+0800] [ALPM] installed python-nose (1.3.7-14)
[2022-09-19T13:28:03+0800] [ALPM] installed lapack (3.10.1-1)
[2022-09-19T13:28:03+0800] [ALPM] installed blas (3.10.1-1)
[2022-09-19T13:28:03+0800] [ALPM] installed libspatialite (5.0.1-3)
[2022-09-19T13:28:02+0800] [ALPM] installed proj (9.0.1-1)
[2022-09-19T13:28:02+0800] [ALPM] installed librttopo (1.1.0-5)
[2022-09-19T13:28:02+0800] [ALPM] installed libfreexl (1.0.6-2)
[2022-09-19T13:28:02+0800] [ALPM] installed geos (3.11.0-1)
[2022-09-19T13:28:02+0800] [ALPM] installed python-bcrypt (4.0.0-1)
[2022-09-19T13:28:02+0800] [ALPM] installed laszip2 (2.2.0-1)
[2022-09-19T13:28:02+0800] [ALPM] installed python-argparse (1.4.0-11)
[2022-09-19T13:28:02+0800] [ALPM] installed ruby-ronn-ng (0.9.1-5)
[2022-09-19T13:28:02+0800] [ALPM] installed ruby-nokogiri (1.13.8-2)
[2022-09-19T13:28:02+0800] [ALPM] installed ruby-mini_portile2 (2.8.0-1)
[2022-09-19T13:28:02+0800] [ALPM] installed ruby-kramdown (2.3.1-2)
[2022-09-19T13:28:02+0800] [ALPM] installed ruby-mustache (1.1.1-2)
[2022-09-19T13:28:02+0800] [ALPM] installed ruby-rdiscount (2.2.0.2-3)
[2022-09-19T13:28:02+0800] [ALPM] installed ispc (1.18.0-3)
[2022-09-19T13:28:02+0800] [ALPM] installed spirv-llvm-translator (14.0.0.r57+g33898cef-1)
[2022-09-19T13:28:02+0800] [ALPM] installed python-psutil (5.9.1-1)




[2022-09-17T13:55:31+0800] [ALPM] installed unicode-cldr-annotations (39.0-1)
[2022-09-17T13:55:30+0800] [ALPM] installed fmt (9.1.0-1)
[2022-09-17T13:55:30+0800] [ALPM] installed xcb-imdkit (1.0.3-1)
[2022-09-17T13:55:30+0800] [ALPM] installed enchant (2.3.3-1)
[2022-09-17T13:33:23+0800] [ALPM] installed pkgconf (1.8.0-1)
[2022-09-17T13:33:23+0800] [ALPM] installed automake (1.16.5-1)
[2022-09-17T13:33:23+0800] [ALPM] installed make (4.3-3)
[2022-09-17T13:33:23+0800] [ALPM] installed flex (2.6.4-3)
[2022-09-17T13:33:23+0800] [ALPM] installed cmake (3.24.1-1)
[2022-09-17T13:33:22+0800] [ALPM] installed rhash (1.4.2-1)
[2022-09-17T13:33:22+0800] [ALPM] installed jsoncpp (1.9.5-2)
[2022-09-17T13:33:22+0800] [ALPM] installed autoconf (2.71-1)
[2022-09-17T13:33:22+0800] [ALPM] installed guile (2.2.7-2)
[2022-09-17T13:33:22+0800] [ALPM] installed gc (8.2.0-3)
[2022-09-17T13:33:22+0800] [ALPM] installed bison (3.8.2-4)
[2022-09-17T13:33:22+0800] [ALPM] installed m4 (1.4.19-1)
[2022-09-17T13:33:22+0800] [ALPM] installed patch (2.7.6-8)
[2022-09-17T13:33:22+0800] [ALPM] installed libuv (1.44.2-1)

[2022-09-17T13:33:22+0800] [ALPM] installed ruby-tmpdir (0.1.2-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-mutex_m (0.1.1-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-logger (1.5.1-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-ipaddr (1.2.4-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-io-wait (0.2.3-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-io-nonblock (0.1.0-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-io-console (0.5.11-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-getoptlong (0.1.1-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-forwardable (1.3.2-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-find (0.1.1-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-fileutils (1.6.0-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-fiddle (1.1.0-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-fcntl (1.0.1-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-etc (1.3.0-2)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-erb (2.2.3-2)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-english (0.7.1-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-drb (2.1.0-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-ruby2_keywords (0.0.5-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-digest (3.1.0-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-did_you_mean (1.6.1-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-delegate (0.2.0-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-csv (3.2.5-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-cgi (0.3.2-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-bigdecimal (3.1.2-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-benchmark (0.2.0-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-base64 (0.1.1-1)
[2022-09-17T13:33:22+0800] [ALPM] installed ruby-abbrev (0.1.0-1)
[2022-09-17T13:33:22+0800] [ALPM] upgraded rubygems (3.3.15-1 -> 3.3.19-1)
[2022-09-17T13:33:22+0800] [ALPM] upgraded rsync (3.2.5pre2-1 -> 3.2.5-2)
[2022-09-17T13:33:22+0800] [ALPM] upgraded python-systemd (234-11 -> 235-1)
[2022-09-17T13:33:22+0800] [ALPM] upgraded python-six (1.16.0-5 -> 1.16.0-6)
[2022-09-17T13:33:22+0800] [ALPM] upgraded python-setuptools (1:61.3.1-1 -> 1:62.3.4-1)
[2022-09-17T13:33:22+0800] [ALPM] upgraded python-validate-pyproject (0.9-1 -> 0.10.1-1)
[2022-09-17T13:33:22+0800] [ALPM] upgraded python-trove-classifiers (2022.8.7-1 -> 2022.8.31-1)
[2022-09-17T13:33:22+0800] [ALPM] upgraded python-pygments (2.12.0-1 -> 2.13.0-1)
[2022-09-17T13:33:21+0800] [ALPM] upgraded python-pip (22.2.2-1 -> 22.2.2-2)
[2022-09-17T13:33:21+0800] [ALPM] upgraded python-pep517 (0.12.0-4 -> 0.13.0-1)
[2022-09-17T13:33:21+0800] [ALPM] upgraded python-ordered-set (4.0.2-6 -> 4.1.0-1)
[2022-09-17T13:33:21+0800] [ALPM] upgraded python-nspektr (0.4.0-1 -> 0.4.0-2)
[2022-09-17T13:33:21+0800] [ALPM] upgraded python-jaraco.text (3.8.1-1 -> 3.9.1-1)
[2022-09-17T13:33:21+0800] [ALPM] installed python-inflect (6.0.0-1)
[2022-09-17T13:33:21+0800] [ALPM] installed python-pydantic (1.10.2-1)
[2022-09-17T13:33:21+0800] [ALPM] installed cython (0.29.32-2)
[2022-09-17T13:33:21+0800] [ALPM] installed python-autocommand (2.2.1-1)

[2022-09-17T13:16:06+0800] [ALPM] installed lib32-mesa-vdpau (22.1.6-1)
[2022-09-17T13:16:06+0800] [ALPM] installed lib32-vulkan-radeon (22.1.6-1)
[2022-09-17T13:16:06+0800] [ALPM] installed lib32-vulkan-intel (22.1.6-1)
[2022-09-17T13:16:06+0800] [ALPM] installed mesa-vdpau (22.1.6-1)
[2022-09-17T13:16:06+0800] [ALPM] installed vulkan-radeon (22.1.6-1)
[2022-09-17T13:16:06+0800] [ALPM] installed vulkan-intel (22.1.6-1)
[2022-09-17T13:16:06+0800] [ALPM] installed xf86-video-nouveau (1.0.17-2)
[2022-09-17T13:16:06+0800] [ALPM] installed xf86-video-intel (1:2.99.917+916+g31486f40-2)
[2022-09-17T13:16:06+0800] [ALPM] installed libxvmc (1.0.13-1)
[2022-09-17T13:16:06+0800] [ALPM] installed xf86-video-amdgpu (22.0.0-1)
[2022-09-17T13:16:06+0800] [ALPM] installed xf86-video-ati (1:19.1.0.r9.g5eba006e-2)
```
</details>

当中有一些我不确定是 ros 相关的依赖，如果不小心删错了咋搞哦。然后就有了下边的思路：把已经安装了的ros打头的包都列出来，然后逐个带依赖卸载

## 过滤之后的名单

查找替换加上正则表达式： `[\(（].*[\)）]`， `[\[].*[\]] `

<details>
<summary>ros打头的名单</summary>

```
ros-noetic-pcl-msgs
ros-noetic-urdfdom-py
ros-noetic-image-pipeline
ros-noetic-image-publisher
ros-noetic-depth-image-proc
ros-noetic-stereo-image-proc
ros-noetic-camera-info-manager
ros-noetic-image-proc
ros-noetic-image-rotate
ros-noetic-joint-state-controller
ros-noetic-controller-interface
ros-noetic-hardware-interface
ros-noetic-nodelet-tutorial-math
ros-noetic-control-toolbox
ros-noetic-turtle-tf
ros-noetic-self-test
ros-noetic-control-msgs
ros-noetic-ros-tutorials
ros-noetic-rospy-tutorials
ros-noetic-camera-calibration
ros-noetic-roscpp-tutorials
ros-noetic-image-geometry
ros-noetic-viz
ros-noetic-joint-state-publisher
ros-noetic-rviz-python-tutorial
ros-noetic-visualization-marker-tutorials
ros-noetic-turtlesim
ros-noetic-rqt-common-plugins
ros-noetic-rqt-reconfigure
ros-noetic-rqt-srv
ros-noetic-rqt-py-console
ros-noetic-rqt-web
ros-noetic-roslint
ros-noetic-rqt-publisher
ros-noetic-rqt-dep
ros-noetic-webkit-dependency
ros-noetic-rqt-launch
ros-noetic-rqt-service-caller
ros-noetic-rqt-shell
ros-noetic-rqt-bag-plugins
ros-noetic-rqt-plot
ros-noetic-ros-base
ros-noetic-qwt-dependency
ros-noetic-ros-core
ros-noetic-ros-comm
ros-noetic-common-msgs
ros-noetic-roslisp
ros-noetic-shape-msgs
ros-noetic-ros
ros-noetic-trajectory-msgs
ros-noetic-roscreate
ros-noetic-rosmake
ros-noetic-rosboost-cfg
ros-noetic-nodelet-core
ros-noetic-rosbash
ros-noetic-rosbag-migration-rule
ros-noetic-roscpp-core
ros-noetic-mk
ros-noetic-bond-core
ros-noetic-rqt-robot-plugins
ros-noetic-nodelet-topic-tools
ros-noetic-rqt-rviz
ros-noetic-rviz
ros-noetic-media-export
ros-noetic-urdf
ros-noetic-map-msgs
ros-noetic-urdf-parser-plugin
ros-noetic-resource-retriever
ros-noetic-laser-geometry
ros-noetic-rosconsole-bridge
ros-noetic-tf2-eigen
ros-noetic-rqt-robot-dashboard
ros-noetic-rqt-robot-monitor
ros-noetic-rqt-bag
ros-noetic-rqt-moveit
ros-noetic-qt-gui-py-common
ros-noetic-rqt-robot-steering
ros-noetic-rqt-pose-view
ros-noetic-rqt-topic
ros-noetic-rqt-runtime-monitor
ros-noetic-rqt-nav-view
ros-noetic-gl-dependency
ros-noetic-rqt-tf-tree
ros-noetic-rqt-graph
ros-noetic-interactive-markers
ros-noetic-qt-dotgraph
ros-noetic-stage-ros
ros-noetic-visualization-msgs
ros-noetic-tf2-geometry-msgs
ros-noetic-stage
ros-noetic-rqt-image-view
ros-noetic-nav-msgs
urdfdom-headers
ros-noetic-smach
ros-noetic-geometry
ros-noetic-tf-conversions
ros-noetic-eigen-conversions
ros-noetic-tf
ros-noetic-tf2-ros
ros-noetic-tf2-py
ros-noetic-tf2
ros-noetic-kdl-conversions
ros-noetic-tf2-msgs
ros-noetic-roswtf
ros-noetic-angles
ros-noetic-rqt-gui-cpp
ros-noetic-qt-gui-cpp
ros-noetic-diagnostic-updater
ros-noetic-rqt-top
ros-noetic-rqt-action
ros-noetic-diagnostic-msgs
ros-noetic-rqt-msg
ros-noetic-rqt-console
ros-noetic-rqt-logger-level
ros-noetic-rqt-gui-py
ros-noetic-rosnode
ros-noetic-rqt-py-common
ros-noetic-rqt-gui
ros-noetic-qt-gui
ros-noetic-python-qt-binding
ros-noetic-bondpy
ros-noetic-realtime-tools
removedpython-pyqt5
ros-noetic-actionlib
ros-noetic-image-view
ros-noetic-compressed-image-transport
ros-noetic-rostopic
ros-noetic-actionlib-msgs
ros-noetic-image-transport
ros-noetic-stereo-msgs
ros-noetic-message-filters
ros-noetic-camera-calibration-parsers
ros-noetic-dynamic-reconfigure
ros-noetic-cv-bridge
ros-noetic-rosservice
ros-noetic-rosmsg
ros-noetic-rosbag
ros-noetic-rosbag-storage
ros-noetic-topic-tools
ros-noetic-rostest
ros-noetic-roslaunch
ros-noetic-nodelet
ros-noetic-rosmaster
ros-noetic-rosparam
ros-noetic-rosout
ros-noetic-rosclean
ros-noetic-pluginlib
ros-noetic-rospy
ros-noetic-class-loader
ros-noetic-rosgraph
ros-noetic-bondcpp
ros-noetic-bond
ros-noetic-smclib
ros-noetic-roscpp
ros-noetic-roslang
ros-noetic-rosconsole
ros-noetic-rosunit
ros-noetic-roslib
ros-noetic-rospack
ros-noetic-rosbuild
ros-noetic-ros-environment
ros-noetic-rosgraph-msgs
ros-noetic-sensor-msgs
ros-noetic-geometry-msgs
ros-noetic-std-msgs
ros-noetic-xmlrpcpp
ros-noetic-roslz4
ros-noetic-cmake-modules
ros-noetic-std-srvs
ros-noetic-message-generation
ros-noetic-geneus
ros-noetic-gencpp
ros-noetic-gennodejs
ros-noetic-message-runtime
ros-noetic-genlisp
ros-noetic-roscpp-serialization
ros-noetic-roscpp-traits
ros-noetic-rostime
ros-noetic-cpp-common
console-bridge
ros-noetic-genpy
ros-noetic-genmsg
ros-noetic-catkin
ros-build-tools
```
</details>

## 卸载脚本

将ros打头的名单，存到 tmp.txt，然后新建脚本文件 removeRos.sh，粘贴下面的内容，然后执行即可（bash removeRos.sh），然后会要求输入一次密码，之后就不用管了

```bash
#!/bin/bash
# file name: removeRos.sh
# usage：逐行读取 file 文件的内容，然后调用pacman卸载包及其依赖包

file='tmp.txt'
i=1
while read line; do
echo Y|sudo pacman -Rs $line
i=$((i+1))
done < $file
```

本来想用 py 写的，想了一下可能不太优雅，换种方式写，现学先写 sh。最后发issue得到恢复是 manjaro 不咋的兼容 AUR 了，作者说不少 manjaro 跑向了 EndeavourOS。其中有我一个。