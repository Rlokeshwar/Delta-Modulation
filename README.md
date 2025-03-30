# Ex No: 04 - DELTA MODULATION

## Aim
To implement and analyze the delta Modulation (DM) process using Python by performing sampling, quantization, and encoding of an analog signal.

## Tools Required
IDE python with scipy and numpy


## Program:
~~~
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, filtfilt
fs = 10000  
f = 10  
T = 1  
delta = 0.1 
t = np.arange(0, T, 1/fs)
message_signal = np.sin(2 * np.pi * f * t)  
encoded_signal = []
dm_output = [0]  
prev_sample = 0
for sample in message_signal:
    if sample > prev_sample:
        encoded_signal.append(1)
        dm_output.append(prev_sample + delta)
    else:
        encoded_signal.append(0)
        dm_output.append(prev_sample - delta)
    prev_sample = dm_output[-1]

demodulated_signal = [0]
for bit in encoded_signal:
    if bit == 1:
        demodulated_signal.append(demodulated_signal[-1] + delta)
    else:
        demodulated_signal.append(demodulated_signal[-1] - delta)
demodulated_signal = np.array(demodulated_signal)
def low_pass_filter(signal, cutoff_freq, fs, order=4):
    nyquist = 0.5 * fs
    normal_cutoff = cutoff_freq / nyquist
    b, a = butter(order, normal_cutoff, btype='low', analog=False)
    return filtfilt(b, a, signal)
filtered_signal = low_pass_filter(demodulated_signal, cutoff_freq=20, fs=fs)
plt.figure(figsize=(12, 6))
plt.subplot(3, 1, 1)
plt.plot(t, message_signal, label='Original Signal', linewidth=1)
plt.legend()
plt.grid()
plt.subplot(3, 1, 2)
plt.step(t, dm_output[:-1], label='Delta Modulated Signal', where='mid')
plt.legend()
plt.grid()
plt.subplot(3, 1, 3)
plt.plot(t, filtered_signal[:-1], label='Demodulated & Filtered Signal', linestyle='dotted', linewidth=1, color='r')
plt.legend()
plt.grid()
plt.tight_layout()
plt.show()

~~~
## Output Waveform:
![image](https://github.com/user-attachments/assets/7cd4814f-4491-4ac1-bba7-78568a308e27)








## Results:
The plot compares the original signal, the modulated signal, and the reconstructed signal for visual analysis.thus delta modulation is successfull in coverting analog to digital signal.
