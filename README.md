# Pulse-Code-Modulation
# Aim
Write a simple Python program for the modulation and demodulation of PCM, and DM.
# Tools required
```
Tools Required
Google Colab
Python
NumPy Library
Matplotlib Library
Internet Connection
Computer / Laptop
````
# Theory
Pulse Code Modulation (PCM) is a widely used digital modulation technique that converts analog signals into digital form for reliable transmission and storage. In this method, a continuous-time analog signal is first converted into a discrete-time signal through sampling. According to the Nyquist Theorem, the sampling frequency must be at least twice the maximum frequency component of the signal to avoid aliasing and ensure accurate reconstruction at the receiver. This sampled signal still has continuous amplitude values, which are then processed further.

The next step is quantization, where each sampled amplitude is approximated to the nearest value from a finite set of levels. This process introduces a small difference between the actual and approximated values, known as quantization error or noise. Quantization can be uniform or non-uniform, depending on how the levels are distributed. After quantization, encoding is performed, where each quantized level is assigned a unique binary code. This results in a stream of binary data that represents the original analog signal and can be easily transmitted through digital communication systems.

PCM systems also include processes at the receiver side, where the digital signal is decoded, reconstructed, and filtered to recover the original analog signal. The quality of the reconstructed signal depends on factors such as sampling rate, number of quantization levels, and channel conditions. PCM offers several advantages, including high noise immunity, ease of multiplexing, and compatibility with modern digital systems. However, it requires a larger bandwidth and involves more complex circuitry compared to analog communication methods. Due to its reliability and efficiency, PCM is extensively used in applications such as digital telephony, audio recording, compact discs, and satellite communication systems.

# Program
```
import numpy as np
import matplotlib.pyplot as plt

# -----------------------------------
# Step 1: Generate Analog Input Signal
# -----------------------------------

fm = 2                  # Message signal frequency (Hz)
fs = 20                 # Sampling frequency (Hz)
duration = 2            # Duration in seconds
n_bits = 3              # Number of bits for PCM

t = np.linspace(0, duration, 1000)     # Continuous time axis
ts = np.arange(0, duration, 1/fs)      # Sampled time axis

# Original analog signal (Sine wave)
x = np.sin(2 * np.pi * fm * t)

# Sampled signal
xs = np.sin(2 * np.pi * fm * ts)

# -----------------------------------
# Step 2: Quantization
# -----------------------------------

L = 2 ** n_bits         # Number of quantization levels
x_min = -1
x_max = 1

delta = (x_max - x_min) / L

# Uniform Quantization
xq = np.round((xs - x_min) / delta) * delta + x_min
xq = np.clip(xq, x_min, x_max)

# -----------------------------------
# Step 3: PCM Encoding
# -----------------------------------

indices = ((xq - x_min) / delta).astype(int)
indices = np.clip(indices, 0, L - 1)

binary_codes = [format(i, f'0{n_bits}b') for i in indices]

print("Sampled Value\tQuantized Value\tPCM Code")
print("------------------------------------------------")
for i in range(len(xs)):
    print(f"{xs[i]:.3f}\t\t{xq[i]:.3f}\t\t{binary_codes[i]}")

# -----------------------------------
# Step 4: Create PCM Pulse Waveform
# -----------------------------------

pcm_bits = ""

for code in binary_codes:
    pcm_bits += code

pcm_wave = [int(bit) for bit in pcm_bits]

bit_time = np.arange(len(pcm_wave))

# -----------------------------------
# Step 5: PCM Decoding (Demodulation)
# -----------------------------------

decoded_indices = np.array([int(code, 2) for code in binary_codes])
decoded_signal = decoded_indices * delta + x_min

# -----------------------------------
# Step 6: Plot All Signals
# -----------------------------------

plt.figure(figsize=(12, 14))

# Original Analog Signal
plt.subplot(5, 1, 1)
plt.plot(t, x)
plt.title("Original Analog Signal")
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.grid(True)

# Sampled Signal
plt.subplot(5, 1, 2)
plt.stem(ts, xs, basefmt=" ")
plt.title("Sampled Signal")
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.grid(True)

# Quantized Signal
plt.subplot(5, 1, 3)
plt.stem(ts, xq, basefmt=" ")
plt.title("Quantized Signal")
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.grid(True)

# PCM Binary Pulse Waveform
plt.subplot(5, 1, 4)
plt.step(bit_time, pcm_wave, where='post')
plt.ylim(-0.2, 1.2)
plt.title("PCM Output Waveform (Binary Pulse Train)")
plt.xlabel("Bit Position")
plt.ylabel("Binary Level")
plt.grid(True)

# Demodulated Signal
plt.subplot(5, 1, 5)
plt.step(ts, decoded_signal, where='mid')
plt.title("PCM Demodulated (Reconstructed) Signal")
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.grid(True)

plt.tight_layout()
plt.show()
```
# Output Waveform

<img width="1173" height="1030" alt="Screenshot 2026-04-27 112033" src="https://github.com/user-attachments/assets/eab9ebe5-ee91-44c8-95ff-d99b1ab87c18" />
<img width="1289" height="278" alt="Screenshot 2026-04-27 112040" src="https://github.com/user-attachments/assets/9f60e302-cfb1-4fb5-931c-6d1a5ff6de5f" />

# Results

Thus the simple Python program for the modulation and demodulation of PCM, and DM is simulated and verified.

