# Cascode_opamp_with_floating_current_mirror
In This project I will show the design of a Cascode opamp amplifier with floating mirror as bias circuit in 65[nm] technology.
A floating current mirror is a current source which is not depends on supply. it is named "floating" because it operates without being referenced to a fixed voltage
in the circuit while still providing the desired current mirroring functionality.

In this design the "floating voltage" is NMOS threshold voltage. 

## Design specification
* VDD = 1.8[V]
* DC Gain ~ 60 dB
* PM > 60 [deg]
* GBW > 6[MHz]
* C_{load} = 5[pF]

## Design explained
The design consists of 2 circuits - amplifier circuit and biasing circuit, both design with PMOS and NMOS cascode structure. 
an elaborated explanation can be found in Ali Hajimiri fine lecture - https://youtu.be/OFOgNsqet8Y?si=ilZbU-Q1IspScnR3

**Design schematic**
![image](https://github.com/user-attachments/assets/7e79850f-80ac-4501-98a8-2715221f0c04)

Let's explain the design from bottom (NMOS) to top (PMOS), starting with the floating mirror design - 
* In order to design the biasing current to be proportional to Vth, such that $I_{bias} = V_{th,nmos}/R$, we will need to drop 2 Vgs voltages on M16 gate (M17, M14 Vgs's) and substract the voltages using M16 such that the voltage on R will be Vth only. so we will size M16 to be 1/4 of M17, and because $Vdsat \propto \sqrt{\frac{1}{W/L}}$ we will be left with only Vth. 
* We will choose R = 30[KOhm] and since Vth = 470[mV], we will have roughly $I_{bias} = 15[uA]$
* M14,M17 are sized as W/L = 10/1, and M16 is sized as W/L = 2.5/1
* M13,M15 acts as cascode for the NMOS current source, for better Rout. they are sized as W/L = 10/1
<img width="400" alt="image" align=left src="https://github.com/user-attachments/assets/da71bf6f-c86e-44dc-b27f-a334ac6d6f4f">


* M11, M12, M9, M10 are PMOS cascode for forcing the current to be equal. they are sized x2 the NMOS cascode such that W/L = 20/1
* M18, M19 is used to mirror current to the amplifier stage, see below
<img width="550" alt="image" align=left src="https://github.com/user-attachments/assets/fd32952f-07f1-4ce8-bd15-0ded17188d0f">


* M0,1 are used as gm stage an therefore biased with low L for higher gm -> W/L = 10/0.1
* M3,4 are cascode gm stage for higher rout, W/L = 50/1
* M2 is the tail current which is biased x2 from the floating current mirror such that $I_{bias} = 30[uA]$ for higher gain -> W/L = 20/1
* M20 (together with M18) are biased as PMOS and NMOS biasing circuit (20/1, 10/1) to copy current for amplifier stage using M2
* M21 is used to bias the gm stage cascode by providing Vgs to M3,M4 gates. it is sized as W/L=2/0.5. M19 provides bias current to M21 and therefore it can be a fraction of Itail (~0.01*Itail)
<img width="700" alt="image" align=left src="https://github.com/user-attachments/assets/62fe5945-7b9e-441c-9046-1c8416fc1b1a">

