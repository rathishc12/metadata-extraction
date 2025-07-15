# metadata-extraction
# 🕵️ OSINT Metadata Extractor

A beginner-friendly Python project that extracts EXIF metadata (including GPS coordinates) from image files and plots them on an interactive map.

![Python](https://img.shields.io/badge/Python-3.8+-blue?logo=python)
![Status](https://img.shields.io/badge/Project-Active-brightgreen)

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

### Code
```
from PIL import Image
from PIL.ExifTags import TAGS, GPSTAGS
import folium

def get_exif_data(image_path):
    image = Image.open(image_path)
    exif_data = {}
    
    if hasattr(image, "_getexif"):
        raw_data = image._getexif()
        if raw_data:
            for tag, value in raw_data.items():
                tag_name = TAGS.get(tag, tag)
                exif_data[tag_name] = value
    return exif_data

def get_decimal_from_dms(dms, ref):
    degrees, minutes, seconds = dms
    result = degrees + (minutes / 60.0) + (seconds / 3600.0)
    if ref in ['S', 'W']:
        result = -result
    return result

def get_gps_info(exif_data):
    gps_info = exif_data.get("GPSInfo", {})
    gps_data = {}
    
    if gps_info:
        lat = gps_info.get(2)
        lat_ref = gps_info.get(1)
        lon = gps_info.get(4)
        lon_ref = gps_info.get(3)

        if lat and lon and lat_ref and lon_ref:
            gps_data['Latitude'] = get_decimal_from_dms(lat, lat_ref)
            gps_data['Longitude'] = get_decimal_from_dms(lon, lon_ref)
    return gps_data

def plot_location_on_map(lat, lon):
    map_osint = folium.Map(location=[lat, lon], zoom_start=16)
    folium.Marker([lat, lon], tooltip="Image Location").add_to(map_osint)
    map_osint.save("image_location.html")
    print("✅ Map saved as image_location.html")

def main():
    file_path = input("📁 Enter image file path: ")
    exif_data = get_exif_data(file_path)
    
    print("\n📸 Extracted Metadata:")
    for key, value in exif_data.items():
        print(f"{key}: {value}")
    
    gps = get_gps_info(exif_data)
    if gps:
        print(f"\n📍 GPS Coordinates: {gps}")
        plot_location_on_map(gps['Latitude'], gps['Longitude'])
    else:
        print("\n❌ No GPS data found.")

if __name__ == "__main__":
    main()
```
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
