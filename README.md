
# Deploying PaddleOCR on Jetson Nano

## Introduction

This guide outlines the process of deploying PaddleOCR on the Jetson Nano, from setting up the JetPack environment to installing the required software components. PaddleOCR is a powerful Optical Character Recognition (OCR) tool, and with the steps provided, you will have it running on your Jetson Nano in no time.

---

## Prerequisites

Before starting, ensure you have the following:
- A Jetson Nano with an appropriate power supply.
- A microSD card (at least 32GB) for the Jetson Nano.
- A computer to flash the JetPack image onto the SD card.

---

## Step 1: Installing JetPack on Jetson Nano

### 1. Download JetPack

Download the latest JetPack image from the official [NVIDIA website](https://developer.nvidia.com/embedded/jetpack). **JetPack 4.6.1** is recommended as it corresponds to CUDA 10.2, cuDNN 8.2.1, and TensorRT 8.2.1.

### 2. Prepare the SD Card

- Use [SD Card Formatter](https://www.sdcard.org/downloads/formatter/) to format your microSD card.
- Flash the JetPack image onto the microSD card using [Balena Etcher](https://www.balena.io/etcher/).

### 3. Boot the Jetson Nano

Insert the flashed SD card into your Jetson Nano and power it on. Follow the on-screen instructions to complete the setup.

---

## Step 2: Environment Preparation

Before installing PaddleOCR, verify that your Jetson Nano is properly configured with **CUDA**, **cuDNN**, and **TensorRT**.

### 1. Set Up CUDA Environment Variables

Open a terminal and install `nano` as a text editor if not already installed:

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt install nano
```

Edit the `.bashrc` file to add CUDA environment variables:

```bash
sudo nano ~/.bashrc
```

Add the following lines at the end of the file to configure the environment variables:

```bash
export CUDA_HOME=/usr/local/cuda-10.2
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
export PATH=/usr/local/cuda/bin:$PATH
```

Save the file and apply the changes:

```bash
source ~/.bashrc
```

### 2. Verify the Installation

To confirm that CUDA, cuDNN, and TensorRT are properly installed, run the following commands:

- **Check CUDA version:**
  ```bash
  nvcc -V
  ```

- **Check cuDNN version:**
  ```bash
  cat /usr/include/cudnn_version.h | grep CUDNN_MAJOR -A 2
  ```

- **Check TensorRT version:**
  ```bash
  dpkg -l | grep TensorRT
  ```

---

## Step 3: Install PaddlePaddle-GPU

Since PaddleOCR requires PaddlePaddle-GPU and the Jetson Nano has an ARM architecture, you will need to install it using the official `.whl` package.

### 1. Install Python 3.7

Install Python 3.7 since the default Python version on JetPack 4.6.1 is 3.6:

```bash
sudo apt install python3.7 python3.7-dev python3-pip
```

### 2. Create a Virtual Environment

To avoid version conflicts, create and activate a virtual environment:

```bash
cd ~
pip3 install virtualenv
virtualenv -p /usr/bin/python3.7 pdl_env
source pdl_env/bin/activate
```

### 3. Install PaddlePaddle-GPU

Download the appropriate `.whl` file for your Jetson Nano from the [PaddlePaddle Official Release Page](https://www.paddlepaddle.org.cn/install/quick).

Once downloaded, install it using `pip`:

```bash
pip3 install paddlepaddle_gpu-2.5.2-cp37-cp37m-linux_aarch64.whl
```

### 4. Verify the Installation

Create a Python script `test.py` to verify PaddlePaddle-GPU installation:

```python
import paddle
print(paddle.__version__)
paddle.utils.run_check()
```

Run the script to ensure everything is installed correctly:

```bash
python3 test.py
```

---

## Step 4: Install PaddleOCR

With PaddlePaddle-GPU installed, you can now proceed to install PaddleOCR.

### 1. Install PaddleOCR

Run the following command in your virtual environment to install PaddleOCR:

```bash
pip3 install paddleocr
```

### 2. Test PaddleOCR

Create a Python script `ocr_test.py` to test the PaddleOCR installation:

```python
from paddleocr import PaddleOCR, draw_ocr

# Initialize PaddleOCR
ocr = PaddleOCR(use_angle_cls=True, lang='en')  # need to run only once to download and load model into memory

# Path to your image
img_path = 'your_image.png'

# Perform OCR
result = ocr.ocr(img_path, cls=True)

# Print the result
print(result)
```

Replace `'your_image.png'` with the path to the image you want to perform OCR on.

Run the script:

```bash
python3 ocr_test.py
```

You should see OCR results printed in the terminal.

---

## Troubleshooting

If you encounter issues, here are some common problems and solutions:

- **Virtual environment not activating:**
  - Make sure to activate the environment with the correct command:
    ```bash
    source pdl_env/bin/activate
    ```

- **Dependency conflicts:**
  - Recreate the virtual environment and reinstall the necessary packages.

- **Power issues:**
  - Ensure that the Jetson Nano is connected to a stable power supply, as performance may degrade under low power.

---

## Conclusion

By following this guide, you should now have PaddleOCR successfully deployed on your Jetson Nano. You can now perform Optical Character Recognition on images using the power of PaddlePaddle on an edge AI device.

For more information on PaddleOCR and its features, visit the [PaddleOCR GitHub Repository](https://github.com/PaddlePaddle/PaddleOCR).

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

---

## Acknowledgements

- [NVIDIA Jetson Nano](https://developer.nvidia.com/embedded/jetson-nano-developer-kit)
- [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR)
- [PaddlePaddle](https://www.paddlepaddle.org/)
- [Balena Etcher](https://www.balena.io/etcher/)
- [SD Card Formatter](https://www.sdcard.org/downloads/formatter/)

```

---

### How to Add this README to GitHub:

1. **Create a Repository on GitHub**: Go to your GitHub account, create a new repository, and name it something like "Jetson-Nano-PaddleOCR-Tutorial".

2. **Add the README**: 
   - In your repository, click on **"Add file"** -> **"Create new file"**.
   - Name the file `README.md`.
   - Paste the entire content of the markdown code provided above.
   - Click on **"Commit new file"** to save the README.

3. **Push Code and Files**: If you have other scripts or project files, you can push them to the repository using Git.

This should provide a clear and structured guide to users visiting your repository, making it easy for them to follow along with the PaddleOCR deployment on the Jetson Nano.
