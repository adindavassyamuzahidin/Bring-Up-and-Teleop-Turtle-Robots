# Praktikum Robotika - Modul 3: TurtleBot3 Hardware Bring-Up and Teleoperation

## Identitas Praktikan
* **Nama** : Adinda Vassya Muzahidin[cite: 2]
* **NIM** : 25/558105/PA/23459[cite: 2]
* **Kelas** : ELA[cite: 2]
* **Dosen Pengampu** : Bakhtiar Alldino Ardi Sumbodo, S.Si., M.Cs[cite: 2]
* **Asisten Praktikum** : Bagus Ananta Wijaya[cite: 2]
* **Tanggal** : 15 September 2026[cite: 2]
* **Laboratorium** : Laboratorium Elektronika Dasar & Instrumentasi Dasar, FMIPA UGM[cite: 2]

---

## Deskripsi Singkat
Repositori ini berisi seluruh luaran dan dokumen praktikum **Modul 3 Robotika** mengenai *bring-up* perangkat keras TurtleBot3 Burger menggunakan ROS 2 Humble dan Docker[cite: 1, 2]. Praktikum ini mencakup konfigurasi jaringan nirkabel multi-mesin (PC-Robot), identifikasi topic/node, uji Quality of Service (QoS), serta pengujian teleoperasi terukur[cite: 1].

## Tujuan Praktikum
1. Menjelaskan arsitektur perangkat keras TurtleBot3 (PC - Raspberry Pi - OpenCR - Aktuator/Sensor)[cite: 1].
2. Mengonfigurasi jaringan dan `ROS_DOMAIN_ID` untuk komunikasi nirkabel multi-mesin[cite: 1].
3. Melakukan *bring-up* robot serta memverifikasi node dan topic aktif[cite: 1].
4. Melakukan teleoperasi terukur dengan batas kecepatan yang aman[cite: 1].
5. Mendiagnosis dan menganalisis masalah komunikasi jaringan (DDS discovery)[cite: 1].

---

## Langkah Operasional

### 1. Konfigurasi Jaringan & Environment (Di Robot via SSH)
```bash
ssh ubuntu@<ip-robot>
export ROS_DOMAIN_ID=35
export TURTLEBOT3_MODEL=burger
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
source /opt/ros/humble/setup.bash
source ~/turtlebot3_ws/install/setup.bash
```[cite: 1, 2]

### 2. Jalankan Bring-Up Robot
```bash
ros2 launch turtlebot3_bringup robot.launch.py
```[cite: 1, 2]

### 3. Verifikasi Topic & Node (Di Development PC / Container)
```bash
ros2 node list
ros2 topic list
```[cite: 1, 2]

### 4. Teleoperasi Terukur
```bash
ros2 run turtlebot3_teleop teleop_keyboard
```[cite: 1, 2]

---

## Ringkasan Hasil Pengujian

### Tabel 3.1 - Inventaris Topic
| Topic | Tipe Pesan | Publisher | Frekuensi (Hz) | Reliability | Fungsi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `/cmd_vel` | `geometry_msgs/msg/Twist` | 0 | 10.784 | RELIABLE | Perintah kecepatan gerak |
| `/scan` | `sensor_msgs/msg/LaserScan` | 1 | 4.272 | BEST_EFFORT | Data pemindaian LiDAR 360° |
| `/odom` | `nav_msgs/msg/Odometry` | 1 | 20.046 | RELIABLE | Odometri pergerakan roda |
| `/imu` | `sensor_msgs/msg/Imu` | 1 | 19.930 | RELIABLE | Orientasi dan akselerasi robot |
| `/joint_states` | `sensor_msgs/msg/JointState` | 1 | 20.191 | RELIABLE | Status sendi/roda |
| `/tf` | `tf2_msgs/msg/TFMessage` | 2 | 33.512 | RELIABLE | Transformasi koordinat frame |
| `/battery_state` | `sensor_msgs/msg/BatteryState` | 1 | 19.949 | RELIABLE | Status dan tegangan baterai |

[cite: 2]

### Tabel 3.2 - Hasil Teleoperasi Terukur
| Eksp | Trial | Nilai Teoretis | Nilai Terukur | Error Absolut | Error Relatif |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **E1 (Lurus)** | 1-3 | 30 cm | 33,5 cm | 3,5 cm | 11,67 % |
| **E2 (Henti)** | 1-3 | – | Realtime | 0 | 0 % |
| **E3 (Rotasi)** | 1-3 | 183° | 195° | 12° | 6,56 % |
| **E4 (Busur)** | 1-3 | R = 20 cm | R = 22 cm | 2 cm | 10,00 % |

[cite: 2]
