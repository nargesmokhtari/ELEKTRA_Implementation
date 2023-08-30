# ELEKTRA_Implementation
The provided code defines a function called `electrocardiomatrix` that takes several input parameters: `distance`, `r_peaks`, `filtered_ecg`, `init_window`, and `peaks_window`. 

def electrocardiomatrix(distance, r_peaks, filtered_ecg, init_window, peaks_window):

This line defines the function `electrocardiomatrix` with the input parameters `distance`, `r_peaks`, `filtered_ecg`, `init_window`, and `peaks_window`.

    init_seg = int(0.2 * distance)
    fin_seg = int(1.5 * distance)

These two lines calculate the values of `init_seg` and `fin_seg` based on the `distance` input parameter. They are integers calculated by multiplying `distance` by 0.2 and 1.5, respectively, and then converting them to integers using the `int()` function.

all_segments = []

This line initializes an empty list called `all_segments`. This list will be used to store segments of the `filtered_ecg` signal.

![image](https://github.com/nargesmokhtari/ELEKTRA_Implementation/assets/126694721/e55e26ce-9ecf-47cd-8d6d-aed418f7eb3d)
