# Cascode_opamp_with_floating_current_mirror
In this project I will show the design of a Cascode opamp amplifier with floating mirror as bias circuit in 65[nm] technology.<br>
A floating current mirror is a current source which is not depends on supply. it is named "floating" because it operates without being referenced to a fixed voltage
in the circuit while still providing the desired current mirroring functionality.

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
* M13,M15 are used as cascode for the NMOS current source, for better Rout. they are sized as W/L = 10/1
<img width="400" alt="image" align=left src="https://github.com/user-attachments/assets/da71bf6f-c86e-44dc-b27f-a334ac6d6f4f">

* M11, M12, M9, M10 are PMOS cascode for forcing the current to be equal. they are sized x2 the NMOS cascode such that W/L = 20/1
* M18, M19 is used to mirror current to the amplifier stage, see below
<img width="550" alt="image" align=left src="https://github.com/user-attachments/assets/fd32952f-07f1-4ce8-bd15-0ded17188d0f">

<br> <br> <br> <br> <br> <br> <br> <br> <br> <br> <br> <br> <br>
* M0,1 are used as gm stage, therefore biased with low L for higher gm -> W/L = 10/0.1
* M3,4 are cascode gm stage for higher rout, W/L = 50/1
* M2 is the tail current which is biased x2 from the floating current mirror such that $I_{bias} = 30[uA]$ for higher gain -> W/L = 20/1
* M20 (together with M18) are biased as PMOS and NMOS biasing circuit (20/1, 10/1) to copy current for amplifier stage using M2
* M21 is used to bias the gm stage cascode by providing Vgs to M3,M4 gates. it is sized as W/L=2/0.5. M19 provides bias current to M21 and therefore it can be a fraction of Itail (~0.01*Itail)
<img width="650" alt="image" align=center src="https://github.com/user-attachments/assets/62fe5945-7b9e-441c-9046-1c8416fc1b1a">
<br> <br>
* M5,6,7,8 are used as cascode PMOS for higher Rout for the amplifier stage. they are sized as W/L = 30/1
<img width="500" alt="image" align=center src="https://github.com/user-attachments/assets/22ef32d2-5dca-499b-ad9a-0d436d6b2a3c">

## Design Simulations
For DC, STB and transient simulations we used the following test bench - 
<img width="600" alt="image" align=center src="https://github.com/user-attachments/assets/9a603302-8335-40d3-9675-6f9c346463fa">
<br>
DC gain is like a regular cascode ("Telescopic") amplifier - $A_v = gm_1*(gm_4 * ro_4 * ro_1)||(gm_8 * ro_8 * ro_7)$, and we have 1 dominant pole (1 stage) therefore PM drops by only 90[deg] - 
![image](https://github.com/user-attachments/assets/90401325-34ce-4c2b-94d7-bd1f20abc97f)

So the circuit is quite stable, and GBW ~ 7[MHz]. we can also see it in transient response (left graph) - 
![image](https://github.com/user-attachments/assets/368d7a0c-0ec5-4193-a70f-7d4a6a9bf6d1)

on the right we can see Vout-Vin (offset voltage error) which is below 150[uV].

**CMRR**
<br>
For CMRR we used the following test bench to calculate Acm and Adiff - 
![image](https://github.com/user-attachments/assets/9f58f9d3-3502-4513-863a-aad24343e836)

Extracted the CMRR as $CMRR = A_{OL}/A_{CM}$
![image](https://github.com/user-attachments/assets/06ceb038-5438-43fb-b720-438616e97f77)
