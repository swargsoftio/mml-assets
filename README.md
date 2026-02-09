# `mml-assets` - Tutorial Assets Conversion Guide

This guide explains how to **convert GIFs to MP4/WebM**, as well as how to **tag and push changes** to GitHub. This process will ensure that your tutorial videos stay small, lightweight, and ready for CDN delivery.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Converting GIFs to MP4 and WebM](#converting-gifs-to-mp4-and-webm)
3. [Batch Conversion for Multiple GIFs](#batch-conversion-for-multiple-gifs)
4. [Tagging and Pushing Changes to GitHub](#tagging-and-pushing-changes-to-github)
5. [Future Conversion of MP4 to WebM](#future-conversion-of-mp4-to-webm)

---

## 1. 🛠 **Prerequisites**

Before starting, make sure you have the following installed:

- **Homebrew** (for macOS package management)
- **ffmpeg** (for video conversion)

### Install ffmpeg via Homebrew

If you don’t have `ffmpeg` installed, run the following:

```bash
brew install ffmpeg
```

---

## 2. 🎥 **Converting GIFs to MP4 and WebM**

To convert your **GIFs** into **MP4** and **WebM** formats, you can use the following commands.

### Command to convert **GIF to MP4**:

```bash
ffmpeg -y -i input.gif \
  -movflags faststart \
  -pix_fmt yuv420p \
  -vf "scale=trunc(iw/2)*2:trunc(ih/2)*2" \
  output.mp4
```

This command:

- Converts the GIF to MP4
- Ensures compatibility across browsers
- Optimizes video size

### Command to convert **GIF to WebM**:

```bash
ffmpeg -y -i input.gif \
  -c:v libvpx-vp9 \
  -b:v 0 \
  -crf 32 \
  -pix_fmt yuv420p \
  output.webm
```

This command:

- Converts the GIF to WebM (which is even smaller than MP4)
- Uses VP9 encoding for better compression

---

## 3. 🧑‍💻 **Batch Conversion for Multiple GIFs**

To batch convert **all GIFs** in a folder to **MP4** and **WebM**, run the following commands:

### Batch Convert **GIF to MP4**:

```bash
for f in *.gif; do
  ffmpeg -y -i "$f" \
    -movflags faststart \
    -pix_fmt yuv420p \
    -vf "scale=trunc(iw/2)*2:trunc(ih/2)*2" \
    "${f%.gif}.mp4"
done
```

### Batch Convert **GIF to WebM**:

```bash
for f in *.gif; do
  ffmpeg -y -i "$f" \
    -c:v libvpx-vp9 \
    -b:v 0 \
    -crf 32 \
    -pix_fmt yuv420p \
    "${f%.gif}.webm"
done
```

This will convert **every `.gif` file** in your current directory to MP4 and WebM formats.

---

## 4. 🏷 **Tagging and Pushing Changes to GitHub**

### 1. **Navigate to your GitHub Repo Folder**

Go to your `mml-assets` repo directory:

```bash
cd ~/path/to/mml-assets
```

### 2. **Check the Status of New/Modified Files**

Check which files have been modified or added (e.g., MP4/WebM):

```bash
git status
```

### 3. **Stage the New Files**

Add the converted files to the staging area:

```bash
git add tutorials/*.mp4 tutorials/*.webm
```

### 4. **Commit Your Changes**

Commit the added files to GitHub with a descriptive message:

```bash
git commit -m "Converted GIFs to MP4/WebM videos"
```

### 5. **Tag Your Changes**

Create a new Git tag to mark this release (e.g., `v1.1.0`):

```bash
git tag v1.1.0
```

This will help you version your assets and easily link to the specific version in the CDN.

### 6. **Push Changes to GitHub**

Push the changes to your main branch:

```bash
git push origin main
```

Push the new tag:

```bash
git push origin v1.1.0
```

---

## 5. 🔄 **Future Conversion of MP4 to WebM**

When you get **MP4 videos** in the future and need to convert them to **WebM**, use the following command:

### Convert MP4 to WebM:

```bash
ffmpeg -i input.mp4 -c:v libvpx-vp9 -b:v 0 -crf 32 -pix_fmt yuv420p output.webm
```

This ensures that:

- MP4 videos are converted to WebM for better performance (especially on mobile).

---

### 💡 **Final Notes**

- Make sure to **version your tags** (e.g., `v1.1.0`, `v1.2.0`) to keep track of changes over time.
- Using **WebM** for small tutorials is highly recommended as it will save bandwidth and load times.
- **Avoid committing GIFs** if they are larger than ~20 MB, as they will hit the GitHub limits.
