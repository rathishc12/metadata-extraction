# metadata-extraction
# 🕵️ OSINT Metadata Extractor

A beginner-friendly Python project that extracts EXIF metadata (including GPS coordinates) from image files and plots them on an interactive map.

![Python](https://img.shields.io/badge/Python-3.8+-blue?logo=python)
![Status](https://img.shields.io/badge/Project-Active-brightgreen)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

---

## 📌 Features

- ✅ Extracts EXIF metadata using `Pillow`
- ✅ Detects GPS coordinates from images
- ✅ Generates an interactive map using `folium`
- ✅ Saves the map as `image_location.html`
- ✅ Works well on Kali Linux (OSINT-focused)

---

## 💻 Tech Stack

- **Python 3.8+**
- [Pillow](https://pypi.org/project/Pillow/)
- [Folium](https://pypi.org/project/folium/)
- [ExifTool](https://exiftool.org/) (Kali has it pre-installed)

---

## 📦 Installation

### Step 1: Clone the repository (or download ZIP)

```bash
git clone https://github.com/rathishc12/osint-metadata.git
cd osint-metadata

```
### Step 2: Set up virtual environment 
```bash
python3 -m venv myenv
source myenv/bin/activate

```
### Step 3: Install dependencies
```bash
pip install pillow folium
sudo apt install libimage-exiftool-perl -y
python3 extractor-metadata.py
