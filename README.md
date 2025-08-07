Overview
-------------
This code processes electrophysiological data recorded using the INTAN system, focusing on experiments involving three ports:
- Port A: PEDOT:PSS/DES cuff (used for stimulation and recording)
- Port B: PEDOT:PSS cuff (used for stimulation and recording)
- Port C: EMG needle electrode (used only during stimulation trials to monitor muscle responses)
The code performs offline data analysis, including signal processing, impedance calculations, and statistical comparisons for baseline and evoked activity or stimulation experiments. Key analysis metrics include signal RMS, signal-to-noise ratio (SNR), spike detection, stimulation artifacts, and impedance modeling. The processed data is saved in a Pickle (PKL) format for each experiment.

Important usage notes:
- For recording trials (spontaneous or evoked activity), only Ports A and B are relevant. Port C can be ignored.
- For stimulation trials, Port C contains EMG signals of interest. Ports A and B were used only for stimulation and may not contain meaningful data.
  
Data Workflow
-----------------
1. Load Data: The code loads the raw .rhs files from the INTAN system for each experiment. Data from all three ports (A, B, C) are read, and baseline or stimulation conditions are identified.
2. Impedance Calculation: Impedance magnitude and phase values are extracted from the INTAN metadata corresponding to baseline recordings. From these values, the equivalent series resistance (R) and capacitance (C) for each electrode are computed using standard RC circuit approximation techniques.

3. Recording signal analysis:
3.1 Bandpass Filtering: Signals are bandpass filtered between 200–3000 Hz to isolate high-frequency activity.
3.2 RMS Calculation: The root mean square (RMS) of the signal is computed for each channel. For pain-evoked activity, RMS is calculated for both baseline and evoked conditions.
3.3 Signal-to-Noise Ratio (SNR): SNR is computed as the ratio of RMS during evoked activity to RMS during baseline.
3.4 Spike Detection: Spikes elicited by mechanical stimulation are detected using a negative threshold of –12 µV. Waveforms are extracted and averaged around the negative peak to characterize the evoked response.

4. Electrical Stimulation Analysis:
4.1 Artifact Detection: Adaptive thresholding is applied to detect stimulation artifacts, with a minimum inter-artifact interval of 1 second.
4.2 Post-Stimulus Analysis: A post-stimulus window (11 ms to 200 ms) is used to capture evoked neural activity while avoiding contamination from the artifact.
4.3 RMS Calculation for Stimulation: RMS values are computed within the post-stimulus window and compared to baseline RMS values to assess the impact of stimulation.

5. Statistical Analysis: For each animal, paired statistical comparisons are made between electrode materials (e.g., PEDOT:PSS vs PEDOT:DES). Parametric t-tests or non-parametric Wilcoxon signed-rank tests are used depending on the normality of the data. Statistical comparisons are performed on impedance and RC values, RMS values, SNR, and muscle stimulation thresholds.

Code Requirements
------------------
- Python 3.10
- Make sure to have the INTAN library installed to handle .rhs files.
