## Time-Domain Features

These features are calculated directly from the raw or cleaned time-series data chunk $x[n]$ without converting it into another domain. They are computationally very cheap, making them ideal first-line features for microcontrollers.

- **Mean (Before PCA):**
    - _What:_ The arithmetic average of all data points in the window.
    - _Why:_ Establishes the static baseline or DC offset of the sensor signal. Extracting it _before_ Principal Component Analysis (PCA) ensures the data is properly centered.
    
- **Max Amplitude:**
    - _What:_ The peak absolute value within the window ($\max(|x[n]|)$).
    - _Why:_ Captures the maximum intensity or physical impact of an event (e.g., a footstep in an accelerometer stream or a sudden noise burst).
    
- **Energy:**
    - _What:_ The sum of squares of the signal values ($\sum x[n]^2$).
    - _Why:_ Represents the total physical power contained in the chunk. It provides an immediate indicator of activity vs. absolute silence/inactivity.
    
- **Signal-to-Noise Ratio (SNR):**
    - _What:_ The ratio of the desired signal power to the background noise power.
    - _Why:_ Quantifies signal quality. If the SNR is too low, the edge system can decide to sleep or drop the chunk rather than waste battery running a prediction on corrupted data.
    
- **Peak Decay:**
    - _What:_ The rate at which the signal amplitude diminishes over time following a peak.
    - _Why:_ Crucial for structural health monitoring or predictive maintenance (e.g., measuring how a mechanical vibration dampens out to detect worn-out components).
    
- **Variance of Peaks:**
    - _What:_ The statistical variance in the heights and intervals of detected local maxima.
    - _Why:_ Measures the rhythm and irregularity of a signal. Highly useful in biometric applications like ECG anomaly detection or gait analysis.
    

## Frequency-Domain & Time-Frequency Features

These transform the time chunk into the frequency domain to isolate rhythmic or harmonic behaviors.

- **Main Frequency (Dominant Frequency):**
    - _What:_ The frequency component exhibiting the highest magnitude after a Fast Fourier Transform (FFT).
    - _Why:_ pinpoints the primary repeating cycle (e.g., identifying motor RPM, or classifying different speech vowels).
    
- **Spectrogram:**
    - _What:_ A 2D visual representation of the spectrum of frequencies as they vary over time, calculated by applying consecutive Short-Time Fourier Transforms (STFT) over sliding sub-windows.
    - _Why:_ It maps a 1D time-series signal into a 2D image matrix. This allows lightweight 2D Convolutional Neural Networks (CNNs) to extract powerful spatiotemporal features for audio recognition (e.g., Keyword Spotting) or complex vibration classification.
    

## Dimensionality Reduction

- **PCA Eigenvalues:**
    - _What:_ The variance explained by each principal component derived from Principal Component Analysis (PCA).
    - _Why:_ Instead of feeding an entire multi-channel raw data chunk into a model, PCA projects the data onto a lower-dimensional space. The eigenvalues tell the edge device which directions hold the most physical information, allowing it to discard low-eigenvalue vectors and vastly shrink the network's input tensor size.
    

## Computer Vision / Image Feature Detection

When dealing with camera inputs on the edge (Vision AI), traditional machine vision features can highlight geometric structures before passing the image to a heavy neural network.

- **Edge Detection:**
    - _What:_ Highlights sharp discontinuities and abrupt changes in pixel intensity (e.g., Sobel or Canny filters).
    - _Why:_ Outlines object boundaries while stripping away color and illumination noise, reducing the input complexity to basic shapes.
    
- **Corner Detection:**
    - _What:_ Identifies points where two or more edge directions intersect (e.g., Harris Corner Detector).
    - _Why:_ Extracts stable, highly invariant keypoints across frames, making it essential for low-power optical flow, spatial orientation, or object tracking.
    
- **Blob Detection:**
    - _What:_ Isolates regions in an image that share similar properties (like brightness, color, or texture) compared to surrounding areas (e.g., Laplacian of Gaussian).
    - _Why:_ Automatically segments prospective target objects or anomalies from a flat background for faster regions-of-interest (RoI) classification.
    
- **Ridge Detection:**
    - _What:_ Captures local thin curves and elongated structures that form a central axis or crest.
    - _Why:_ Ideal for specific tracking applications on the edge, such as fingerprint scanner reading, road lane tracking, or medical vessel segmentation.