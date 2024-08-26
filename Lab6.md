# ASIC Lab 6 Report (27/08/2024)

In this lab, we have simulated the same sum 1 to 9 program as written in previous lab and compared the output waveforms of gtkwave and makerchip platform.

## Simulation of Sum 1 to 9 program in Sandpiper and GTKWave

Below are the steps:

**Step 1** : Install the required packages

To install the python3, Sandpiper and gtkwave packages with the below commands

```

sudo apt install make python python3 python3-pip git iverilog gtkwave
sudo apt-get install python3-venv
pip3 install pyyaml click sandpiper-saas
python3 -m venv .venv
source ~/.venv/bin/activate
sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io
sudo chmod 666 /var/run/docker.sock

```

**Step 2** : Clone the git

Next, clone the git with the below command.

```

git clone https://github.com/manili/VSDBabySoC.git

```

**Step 3** : Replacing the ```.tlv``` file

In the cloned git, navigate to VSDBabySoC/src/module and replace the code present in rvmyth.tlv to the code for sum 1 to 9.

**Step 4** : Navigate to cd VSDBabySoC and convert the ```.tlv``` file to ```.v``` 

The code is written in transaction level verilog language. To simulate it needs to be converted to verilog format with the following command.

```

sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/

```

**Step 5** : Create the ```.vcd``` file to view the waveform

Post conversion of the verilog code from ```.tlv``` file to ```.v``` , ```.vcd``` file needs to be created with the following command.

```

make pre_synth_sim

```

**Step 6** : Compile and simulate the verilog code

Simulate the verilog code in iverilog simulator

```

iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module

```

**Step 7** : View the output waveform

Finally, to view the output waveform in the earlier created ```.vcd``` file run the below commands.

```

cd output
./pre_synth_sim.out

```

GTKWave Output Waveform:

![image](https://github.com/user-attachments/assets/c6807f19-5085-489b-a94f-dcada9b7868e)

Below is the makerchip output waveform for comparison.

Makerchip Output Waveform:

![image](https://github.com/user-attachments/assets/39bc031b-920c-4e40-a795-0536dc311974)

**Conclusion** : By comparing the output waveforms of both gtkwave and makerchip we can conclude that the acheived output matches the expectation i.e., the sum of numbers from 1 to 9 which is 45 in decimal or 2D in hexadecimal format.

</details>
