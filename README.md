# AAE5303 Environment Setup Report — Template for Students

> **Important:** Follow this structure exactly in your submission README.  
> Your goal is to demonstrate **evidence, process, problem-solving, and reflection** — not only screenshots.

---

## 1. System Information

**Laptop model:**  
Lenovo 82WM

**CPU / RAM:**  
AMD Ryzen 9 7945HX with Radeon Graphics, 16GB RAM

**Host OS:**  
Windows 11

**Linux/ROS environment type:**  
- [ ] Dual-boot Ubuntu
- [x] WSL2 Ubuntu
- [ ] Ubuntu in VM (UTM/VirtualBox/VMware/Parallels)
- [ ] Docker container
- [ ] Lab PC
- [ ] Remote Linux server

---

## 2. Python Environment Check

### 2.1 Steps Taken

Describe briefly how you created/activated your Python environment:

**Tool used:**  
venv

**Key commands you ran:**
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

**Any deviations from the default instructions:**  
Installed missing packages on Ubuntu and in the venv:
- `sudo apt install -y python3-venv`
- `python -m pip install catkin_pkg`

### 2.2 Test Results

Run these commands and paste the actual terminal output (not just screenshots):

```bash
python scripts/test_python_env.py
```

**Output:**
```
========================================
AAE5303 Environment Check (Python + ROS)
Goal: help you verify your environment and understand what each check means.
========================================

Step 1: Environment snapshot
  Why: We capture platform/Python/ROS variables to diagnose common setup mistakes (especially mixed ROS env).
Step 2: Python version
  Why: The course assumes Python 3.10+; older versions often break package wheels.
Step 3: Python imports (required/optional)
  Why: Imports verify packages are installed and compatible with your Python version.
Step 4: NumPy sanity checks
  Why: We run a small linear algebra operation so success means more than just `import numpy`.
Step 5: SciPy sanity checks
  Why: We run a small FFT to confirm SciPy is functional (not just installed).
Step 6: Matplotlib backend check
  Why: We generate a tiny plot image (headless) to confirm plotting works on your system.
Step 7: OpenCV PNG decoding (subprocess)
  Why: PNG decoding uses native code; we isolate it so corruption/codec issues cannot crash the whole report.
Step 8: Open3D basic geometry + I/O (subprocess)
  Why: Open3D is a native extension; ABI mismatches can segfault. Subprocess isolation turns crashes into readable failures.
Step 9: ROS toolchain checks
  Why: The course requires ROS tooling. This check passes if ROS 2 OR ROS 1 is available (either one is acceptable).
  Action: building ROS 2 workspace package `env_check_pkg` (this may take 1-3 minutes on first run)...
  Action: running ROS 2 talker/listener for a few seconds to verify messages flow...
Step 10: Basic CLI availability
  Why: We confirm core commands exist on PATH so students can run the same commands as in the labs.

=== Summary ===
✅ Environment: {
  "platform": "Linux-6.6.87.2-microsoft-standard-WSL2-x86_64-with-glibc2.35",
  "python": "3.10.12",
  "executable": "/home/yd/aae5303-env-check/.venv/bin/python",
  "cwd": "/home/yd/aae5303-env-check",
  "ros": {
    "ROS_VERSION": "2",
    "ROS_DISTRO": "humble",
    "ROS_ROOT": null,
    "ROS_PACKAGE_PATH": null,
    "AMENT_PREFIX_PATH": "/home/yd/ros2_ws/install/pgo:/home/yd/ros2_ws/install/localizer:/home/yd/ros2_ws/install/hba:/home/yd/ros2_ws/install/interface:/home/yd/ros2_ws/install/fastlio2:/home/yd/ws_livox/install/livox_ros_driver2:/opt/ros/humble",
    "CMAKE_PREFIX_PATH": "/home/yd/ros2_ws/install/pgo:/home/yd/ros2_ws/install/localizer:/home/yd/ros2_ws/install/hba:/home/yd/ros2_ws/install/interface:/home/yd/ros2_ws/install/fastlio2:/home/yd/ws_livox/install/livox_ros_driver2"
  }
}
✅ Python version OK: 3.10.12
✅ Module 'numpy' found (v2.2.6).
✅ Module 'scipy' found (v1.15.3).
✅ Module 'matplotlib' found (v3.10.8).
✅ Module 'cv2' found (v4.12.0).
✅ Module 'rclpy' found (vunknown).
✅ numpy matrix multiply OK.
✅ numpy version 2.2.6 detected.
✅ scipy FFT OK.
✅ scipy version 1.15.3 detected.
✅ matplotlib backend OK (Agg), version 3.10.8.
✅ OpenCV OK (v4.12.0), decoded sample image 128x128.
✅ Open3D OK (v0.19.0), NumPy 2.2.6.
✅ Open3D loaded sample PCD with 8 pts and completed round-trip I/O.
✅ ROS 2 CLI OK: /opt/ros/humble/bin/ros2
✅ ROS 1 tools not found (acceptable if ROS 2 is installed).
✅ colcon found: /usr/bin/colcon
✅ ROS 2 workspace build OK (env_check_pkg).
✅ ROS 2 runtime OK: talker and listener exchanged messages.
✅ Binary 'python3' found at /home/yd/aae5303-env-check/.venv/bin/python3

All checks passed. You are ready for AAE5303
✅ Python + ROS environment checks: PASS (11.7s)

```

```bash
python scripts/test_open3d_pointcloud.py
```

**Output:**
```
✅✅ Loading /home/yd/aae5303-env-check/data/sample_pointcloud.pcd ...
✅ Loaded 8 points.
   ✅ Centroid: [0.025 0.025 0.025]
   ✅ Axis-aligned bounds: min=[0. 0. 0.], max=[0.05 0.05 0.05]
✅ Filtered point cloud kept 7 points.
✅ Wrote filtered copy with 7 points to /home/yd/aae5303-env-check/data/sample_pointcloud_copy.pcd
   ✅ AABB extents: [0.05 0.05 0.05]
   ✅ OBB  extents: [0.08164966 0.07071068 0.05773503], max dim 0.0816 m
Open3D point cloud pipeline looks good.
✅ Open3D point cloud pipeline: PASS (1.4s)

✅✅ Cleaned up 1 generated file(s) in data/.

```

**Screenshot:**  

![Python Tests Passing](images/py%26pointcloud%20test.png)

---

## 3. ROS 2 Workspace Check

### 3.1 Build the workspace

Paste the build output summary (final lines only):

```bash
source /opt/ros/humble/setup.bash
colcon build
```

**Expected output:**
```
Summary: 1 package finished [x.xx s]
```

**Your actual output:**
```
Summary: 1 package finished [13.9s]
```

### 3.2 Run talker and listener

Show both source commands:

```bash
source /opt/ros/humble/setup.bash
source install/setup.bash
```

**Then run talker:**
```bash
ros2 launch env_check_pkg env_check.launch.py
```

**Output (3–4 lines):**
```
[talker-1] [INFO] [1768631278.144079777] [env_check_pkg_talker]: Publishing: 'AAE5303 hello #0'
[listener-2] [INFO] [1768631278.144246380] [env_check_pkg_listener]: I heard: 'AAE5303 hello #0'
[talker-1] [INFO] [1768631278.646784949] [env_check_pkg_talker]: Publishing: 'AAE5303 hello #1'
[listener-2] [INFO] [1768631278.646959092] [env_check_pkg_listener]: I heard: 'AAE5303 hello #1'
```

**Run listener:**
```bash
ros2 launch env_check_pkg env_check.launch.py
```

**Output (3–4 lines):**
```
[listener-2] [INFO] [1768631278.144246380] [env_check_pkg_listener]: I heard: 'AAE5303 hello #0'
[listener-2] [INFO] [1768631278.646959092] [env_check_pkg_listener]: I heard: 'AAE5303 hello #1'
[listener-2] [INFO] [1768631279.149812463] [env_check_pkg_listener]: I heard: 'AAE5303 hello #2'
[listener-2] [INFO] [1768631279.652873845] [env_check_pkg_listener]: I heard: 'AAE5303 hello #3'
```

**Alternative (using launch file):**
```bash
ros2 launch env_check_pkg env_check.launch.py
```

**Screenshot:**  

![Talker and Listener Running](images/ros%20test.png)

---

## 4. Problems Encountered and How I Solved Them

> **Note:** Write 2–3 issues, even if small. This section is crucial — it demonstrates understanding and problem-solving.

### Issue 1: `python3 -m venv .venv` failed with ensurepip not available

**Cause / diagnosis:**  
The `python3-venv` package was missing on Ubuntu, so `ensurepip` could not create the virtual environment.

**Fix:**  
Installed the required venv package.

```bash
sudo apt update
sudo apt install -y python3-venv
```

**Reference:**  
Terminal error message and Ubuntu package hint.

---

### Issue 2: `colcon build` failed with `ModuleNotFoundError: No module named 'catkin_pkg'`

**Cause / diagnosis:**  
`colcon` used the active venv Python, which did not have `catkin_pkg` installed.

**Fix:**  
Installed the missing Python package inside the venv, then rebuilt.

```bash
python -m pip install catkin_pkg
```

**Reference:**  
AI assistant and error message.

---

## 5. Use of Generative AI (Required)

Choose one of the issues above and document how you used AI to solve it.

### 5.1 Exact prompt you asked

**Your prompt:**
```
colcon build 报错：ModuleNotFoundError: No module named 'catkin_pkg'，该怎么解决？
```

### 5.2 Key helpful part of the AI's answer

**AI's response (relevant part only):**
```
在当前虚拟环境里安装 catkin_pkg：
python -m pip install catkin_pkg
然后重新运行 colcon build。
```

### 5.3 What you changed or ignored and why

**Your explanation:**  
I followed the suggestion exactly and installed `catkin_pkg` inside the active venv because the build system was using that Python. I did not need to change any ROS settings after the package was installed.

### 5.4 Final solution you applied

```bash
python -m pip install catkin_pkg
colcon build --packages-select env_check_pkg
```

**Why this worked:**  
`ament_cmake` depends on `catkin_pkg` during package parsing, and installing it in the active environment resolved the import error.

---

## 6. Reflection (3–5 sentences)

**Your reflection:**

This setup showed me how fragile robotics environments can be when a single missing dependency breaks the toolchain. I learned to read build errors carefully and match them to the Python environment that is actually being used. Using WSL2 with ROS2 is convenient, but I need to be disciplined about which environment is sourced and which venv is active. Next time I will verify prerequisites earlier and keep a short checklist of required packages. Overall I feel more confident debugging ROS/Python setup issues.

---

## 7. Declaration

✅ **I confirm that I performed this setup myself and all screenshots/logs reflect my own environment.**

**Name:**  
Han Minghe

**Student ID:**  
25127724G

**Date:**  
2026/1/22

---

## Submission Checklist

Before submitting, ensure you have:

- [x] Filled in all system information
- [x] Included actual terminal outputs (not just screenshots)
- [x] Provided at least 2 screenshots (Python tests + ROS talker/listener)
- [x] Documented 2–3 real problems with solutions
- [x] Completed the AI usage section with exact prompts
- [x] Written a thoughtful reflection (3–5 sentences)
- [x] Signed the declaration

---

**End of Report**

