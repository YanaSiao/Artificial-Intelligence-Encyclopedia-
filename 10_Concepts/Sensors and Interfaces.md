Sensors acquire measurements from the environment or humans and generate data streams in various formats. The memory required to store this data depends on the sampling rate, resolution (bit depth), and the number of channels.

---
### 1. Acoustic and Vibration Sensors (Audio)

These sensors measure vibrations traveling through media like air (microphones), water (hydrophones), or ground (seismometers). They typically produce audio data describing pressure variations.

- **Memory Calculation Formula**:    $$\text{Memory (bytes)} = \text{Length (s)} \times \text{Sampling Rate (Hz)} \times \text{Bytes per Sample} \times \text{Channels}$$
- **Key Details**:
    - **Sampling Frequency**: Crucial for specific applications (e.g., Birdsog detection at 2kHz).
    - **Quantization**: Usually represented in bits (e.g., 8-bit, 16-bit, or 32-bit floating point).

- **Example**: A 10s audio signal sampled at 100Hz, 32-bit (4 bytes), in stereo (2 channels) requires **8 KB** ($10 \times 100 \times 4 \times 2 = 8,000$ bytes).

### 2. Visual and Scene Sensors (Image & Video)

These capture light using a grid of sensor elements to represent an entire scene. This category includes low-power cameras, LIDAR, and RADAR.

- **Image Memory Formula**:     $$\text{Memory (bytes)} = \text{Width} \times \text{Height} \times \text{Bytes per Pixel} \times \text{Channels}$$
    - _Example_: A 128x128 RGB image (3 channels) at 1 byte per pixel requires **49 KB**.

- **Video Memory Formula**: $$\text{Memory (bytes)} = \text{Image Memory} \times \text{Frame Rate (fps)} \times \text{Length (s)}$$
    - _Example_: A 10s video at 30 fps (128x128 RGB, 1 byte/pixel) requires **14.7 MB** uncompressed.

- **Key Details**:
    - **Features**: Color channels (Grayscale vs. RGB), spectral response (e.g., Infrared), pixel size, and frame rate.

### 3. Motion and Position Sensors

These measure an object's physical movement or its location in space. Measurements are typically represented as **Time Series** data.

- **Memory Calculation**: Memory requirement is $n$ bits per sample.
    
- **Key Sensor Types**:
    - **Accelerometer**: Measures change in velocity (acceleration) across axes.
    - **Gyroscope**: Measures the rate of rotation.
    - **Time of Flight (ToF)**: Uses electromagnetic emissions to measure distance.
    - **GPS**: Satellite-based location tracking.

### Radio-based Spatial Sensors (UWB & Radar)

- **Definition**: **Ultra-Wideband (UWB)** is defined by its massive bandwidth rather than a specific frequency. Technically, a signal is considered UWB if its bandwidth exceeds **500 MHz** or **20% of its center frequency**. UWB is strictly a **short-range** technology (typically <100 meters). It is not meant for long-distance data transmission like cellular networks.
    
- **Edge AI Use Case**: the **TinyML for presence detection** and **health/sport monitoring**.
    
- **Advantages**: 
	- **Centimeter-Level Accuracy**: It provides precise location and distance measurement with an accuracy of **10–30 cm**, significantly better than Wi-Fi (meters).
	    
	- **Time of Flight (ToF)**: UWB calculates distance by measuring the actual **travel time** of pulses at the speed of light, rather than relying on signal strength (RSSI), which is easily blocked or reflected.
	    
	- **High Robustness**: It is highly resistant to the "multipath effect" (reflections off walls), making it ideal for indoor environments.
	    
	- **Security**: Because it uses precise timing, it is nearly impossible to "spoof" or perform "relay attacks" (unlike traditional keyless entry systems).
	    
	- **Privacy-Preserving**: In Edge AI, UWB radar is used for behavior recognition and presence detection because it can "see" movement and vital signs (like breathing) without using cameras or microphones.
	   
- **Comparison**: Unlike conventional radio (like Wi-Fi or Bluetooth) that modulates a continuous "sine wave," UWB transmits information by generating extremely short radio pulses—typically **nanoseconds** in length—across a wide range of frequencies.
### 4. Force and Tactile Sensors

Used to facilitate user interaction or measure mechanical strain and liquid/gas flow. These also typically produce **Time Series** data.

- **Key Sensor Types**:
    - **Buttons/Switches**: Binary signals (1 bit).
    - **Capacitive Touch**: Measures contact by conductive objects (like fingers).
    - **Strain Gauges & Load Cells**: Measure deformation and physical load.
    - **Flow & Pressure Sensors**: Measure rates in liquids/gases or environmental pressure.

### 5. Optical, Electromagnetic, and Radiation Sensors

These measure electromagnetic radiation, magnetic fields, and electrical properties.

- **Key Sensor Types**:
    - **Photosensors/Color Sensors**: Detect light wavelengths or precise surface colors.
    - **Magnetometer**: A digital compass measuring magnetic field strength/direction.
    - **Inductive Proximity**: Detects nearby metal via electromagnetic fields.
    - **Current and Voltage Sensors**: Monitor electrical signals.

#### The Differences between Visual and Optical Sensors 

| **Feature**       | **Visual & Scene Sensors**                                 | **Optical & EM Sensors**                              |
| ----------------- | ---------------------------------------------------------- | ----------------------------------------------------- |
| **Primary Goal**  | Capture an entire scene/environment.<br>                   | Measure specific properties of light/fields.          |
| **Output Format** | Grid-based (Images, Video, Point Clouds).<br>              | Chronological (Time Series).                          |
| **Edge AI Task**  | Object detection, gesture recognition, presence detection. | Color recognition, composition analysis, orientation. |
| **Contact**       | Always passive/contactless.                                | Can be passive or require proximity.                  |
### 6. Environmental, Biological, and Chemical Sensors

These monitor ambient conditions or human physiological signals.

- **Key Sensor Types**:
    - **Environmental**: Temperature, humidity, gas ($CO_2$), and particulate matter (pollution).
    - **Biosignals**: ECG (heart) and EEG (brain) activity.
    - **Chemical**: Presence or concentration of specific chemicals.