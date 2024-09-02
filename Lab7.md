<details>
<summary>Lab 7</summary>
<br>

# ASIC Lab 7 Report (29/08/2024)

In this lab, we have used the peripherals namely PLL (Phase Locked Loop) and DAC (Digital to Analog Converter) to convert the digital output obtained in the previous lab from digital format to analog format.

## Definitions

- **Phase Locked Loop:** A **Phase-Locked Loop (PLL)** is an electronic circuit that synchronizes the phase of an output signal with a reference signal by continuously adjusting its frequency. It is widely used in applications like frequency synthesis, clock recovery, and demodulation to maintain signal stability and synchronization.
  
- **Digital to Analog Converter:** A **Digital-to-Analog Converter (DAC)** is an electronic device that converts digital signals, typically binary, into analog voltages or currents. It is commonly used in applications like audio playback, where digital audio data is converted to analog signals for speakers or other output devices.

Now, we need to run the ```rvmyth.v``` file, copy the output to ```.vcd``` file and observe the obtained waveform in GTKWave.

Below is the list of commands to use.

```
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd
```

Terminal Screenshots:

![image](https://github.com/user-attachments/assets/96f0bb4f-3dd5-4d6a-bd5d-2cd20495f753)

Output Waveform Screenshot:

![image](https://github.com/user-attachments/assets/9506b968-2500-485d-a5b4-5e2136721ca5)

</details>
