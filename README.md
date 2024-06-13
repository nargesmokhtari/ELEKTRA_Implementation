#ELEKTRA: ELEKTRokardiomatrix application to biometric identification with convolutional neural networks
#Author links open overlay panelCaterina Fuster-Barceló, Pedro Peris-Lopez, Carmen Camara
# FOR PRE PROCESSING PART WE HAVE:
The provided code defines a function called `electrocardiomatrix` that takes several input parameters: `distance`, `r_peaks`, `filtered_ecg`, `init_window`, and `peaks_window`. 

def electrocardiomatrix(distance, r_peaks, filtered_ecg, init_window, peaks_window):

This line defines the function `electrocardiomatrix` with the input parameters `distance`, `r_peaks`, `filtered_ecg`, `init_window`, and `peaks_window`.

    init_seg = int(0.2 * distance)
    fin_seg = int(1.5 * distance)

These two lines calculate the values of `init_seg` and `fin_seg` based on the `distance` input parameter. They are integers calculated by multiplying `distance` by 0.2 and 1.5, respectively, and then converting them to integers using the `int()` function.

all_segments = []

This line initializes an empty list called `all_segments`. This list will be used to store segments of the `filtered_ecg` signal.

for peak in r_peaks[init_window:init_window + peaks_window]:

This line starts a `for` loop that iterates over a portion of the `r_peaks` list, specifically from the index `init_window` to `init_window + peaks_window`. The loop variable is `peak`.

if peak - init_seg < 0:
            segment = filtered_ecg[0:peak + fin_seg]
        else:
            segment = filtered_ecg[peak - init_seg:peak + fin_seg]

This `if-else` statement checks whether the current `peak` value minus `init_seg` is less than 0. If it is, it selects a segment of the `filtered_ecg` signal from the beginning of the signal (`filtered_ecg[0]`) to the `peak` value plus `fin_seg`. Otherwise, it selects a segment from `peak - init_seg` to `peak + fin_seg`.

all_segments.append(segment[:,np.newaxis])

This line appends the selected `segment` to the `all_segments` list. `[:, np.newaxis]` is used to reshape the segment into a column vector before appending it.


if all_segments[0].shape[0] < all_segments[1].shape[0]:
        zeros = np.zeros(int(all_segments[1].shape[0])-int(all_segments[0].shape[0]))[:, np.newaxis]
        new_segment = np.concatenate((zeros, all_segments[0]))
        all_segments[0] = new_segment

This block of code checks if the number of rows (shape[0]) of the first segment (`all_segments[0]`) is less than the number of rows of the second segment (`all_segments[1]`). If so, it calculates the difference and creates a column vector of zeros with that length (`zeros`). Then, it concatenates the zeros with the first segment and replaces the first segment in `all_segments` with the new concatenated segment.

try:
        ecm = np.concatenate(all_segments, 1)
    except ValueError:
        return None

This `try-except` block attempts to concatenate all the segments in `all_segments` along the second axis (axis 1). If the concatenation is successful, it assigns the result to the variable `ecm`. If a `ValueError` is raised during the concatenation (e.g., if the segments have incompatible
shapes), `None` is returned.

return ecm.T

This line returns the transpose of `ecm`. `ecm.T` swaps the rows and columns of `ecm`, effectively transposing the matrix.

Overall, this function takes input parameters related to ECG (electrocardiogram) data, extracts segments based on peak locations, performs some adjustments, concatenates the segments, and returns the resulting matrix.

 `normalize(signal)`:
   - This function takes a signal as input and returns the normalized version of that signal.
   
   - It defines the range of the normalized signal as [-1, 1] (`a, b = -1, 1`).
   
   - It calculates the range of the input signal (`c`) by subtracting the minimum value from the maximum value.
   
   - It then calculates the normalized signal by subtracting the minimum value of the input signal and dividing it by the range.
   
   - Finally, it scales the normalized signal to the desired range by multiplying it with `c` and adding `a`.
   
`process_ecg(unfiltered_ecg, fs)`:
   - This function processes an electrocardiogram (ECG) signal using the Pan-Tompkins algorithm.
   
   - It takes the unfiltered ECG signal (`unfiltered_ecg`) and the sampling frequency (`fs`) as inputs.
   
   - It applies several steps of the Pan-Tompkins algorithm to the input signal, including bandpass filtering, differentiation, rectification, and integration.
   
   - It initializes the algorithm and detects the R-peaks (R-wave peaks) in the ECG signal.
   
   - It performs various checks and adjustments to identify the definitive R-peaks.
   
   - It converts the definitive peak values to integer type and applies a correction.
   
   - Finally, it returns the corrected R-peaks and the filtered signal.
   
 `peak_distance(r_peaks)`:
 
   - This function calculates the mean distance between all peaks (R-peaks) in an ECG signal.
   
   - It takes the R-peaks as input (`r_peaks`).
   
   - It iterates over the R-peaks and calculates the distance between each peak and the next one.
   
   - It skips any distance that exceeds twice the mean distance plus two times the standard deviation.
   
   - It returns the mean distance between the peaks.
   
 `save_ecm(norm_ecm, train_filled, test_filled, train_ecms, test_ecms, path_test, path_train, key, i, f, j)`:
 
   - This function saves the electrocardiogram (ECG) plot in different paths based on certain conditions.
   
   - It takes several parameters as input, including `norm_ecm` (normalized ECG signal), `train_filled`, `test_filled` (flags indicating whether the training or test set is already filled), `train_ecms`, `test_ecms` (maximum number of ECG plots for training and testing), `path_test`, `path_train` (paths for saving the test and training ECG plots), `key`, `i`, `f`, `j` (loop variables).
   
   - It generates a random number between 0 and 1 (`a`).
   
   - If `a` is less than or equal to 0.2 or the training set is already filled (`train_filled`), it saves the plot in the test set path (`path_test`).
   
   - If `a` is greater than 0.2 or the test set is already filled (`test_filled`), it saves the plot in the training set path (`path_train`).
   
   - It increments the counters (`j` and `f`) based on the saved ECG plots and checks if the respective maximum limits (`test_ecms` and `train_ecms`) are reached.
   
   - Finally, it closes the plot and returns the updated counters (`j` and `f`).

