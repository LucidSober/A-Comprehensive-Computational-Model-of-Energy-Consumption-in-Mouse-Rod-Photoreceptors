# A-Comprehensive-Computational-Model-of-Energy-Consumption-in-Mouse-Rod-Photoreceptors
The computational model successfully advances the field from a qualitative description of photoreceptor energy metabolism to a quantitative, mechanistic, and predictive framework.
By creating an unprecedented "closed-loop" system, the model seamlessly integrates molecular information from gene expression, the biophysical principles of ion movement, the cell's overall ion homeostasis, and the final ATP consumption. This integration allows it to provide profound, and sometimes counterintuitive, insights, the most striking of which is the revelation of the dual role of the hyperpolarization-activated current (Ih) as the main energy consumer under light conditions.
It might be a prerequisite to rationally design and evaluate the next generation of therapeutic strategies aimed at protecting our retina,this mathematical model aims to address such very issue: quantitatively clarifying the specific composition of ATP consumption factors in mouse Photoreceptors cells to maintain ionic homeostasis under both dark and light conditions.
### **Rod Photoreceptor Model Run Instructions**

This guide will assist you in successfully running and using the provided MATLAB rod photoreceptor model.

#### 1. Prerequisites

Before running the model, please ensure you have completed the following preparations:

  File Preparation: Ensure that all the following `.m` files are located in the same folder. This is essential for the model to run correctly:

      main.m
      derivatives.m
      init_parameters.m
      init_state_variables.m
      calculate_currents.m
      phototransduction_cascade.m
      plot_final_results_figure2.m
      calculate_ATP_consumption_accurate.m

    Helper Script (Optional):

       check_initial_conditions.m`: Can be used to verify the reasonability of the initial parameter settings.
      
#### 2. Running the Simulation

1.  Open MATLAB.
2.  Navigate to the File Directory: In the MATLAB interface, use the address bar or the `cd` command in the Command Window to change the current working directory to the folder containing all the `.m` files.
    ```matlab
    % Example: cd 'C:\Your\Folder\Path\To\Model_Files'
    cd 'path\to\your\model_files' 
    ```
3.  Execute the Main Script: In the MATLAB Command Window, type the name of the main script, `main`, and press the **Enter** key.
    or open the main script manually and click the green button to run.
	

The script will begin execution automatically. No further actions are required.

#### 3. Expected Output

As the script runs, you will observe the following outputs:

1.  **Command Window Output**:

       The script will first print progress messages about initialization and finding the steady state.
        ```
        1. Initializing parameters and state variables...
        2. Finding the steady state in dark conditions...
        ```
      This will be followed by a "Steady-State Check," which compares the model's calculated dark-adapted steady-state values against expected values.
      The script will then begin simulating the response to different light intensities and will print its progress.
        ```
        3. Starting light response simulations...
           Simulating 1/8: Light Intensity = 1.7 photons/um^2/s
           ...
           Simulating 8/8: Light Intensity = 4630.0 photons/um^2/s
        ```
      After the simulations are complete, a detailed analysis of key variables at specific time points (e.g., 0.5s, 0.52s) will be printed.
      Finally, messages indicating that plotting and the simulation are complete will appear.
        ```
        5. Plotting results...
        Simulation complete!
        ```

2.  Graphical Output:

       Upon completion, a MATLAB figure window will automatically open with the title "Figure 2 Reproduction: Electrophysiological Responses to Light Flashes".
      This figure contains a **6x4 grid of 24 subplots**, showing the time courses of various electrophysiological and ionic concentration variables (e.g., membrane potential, various ion currents, calcium concentrations) for each simulated light intensity.

#### 4. Customization Guide

You can easily modify key parameters to suit your research needs. All recommended modifications should be made in the `main.m` file.

  Modifying Light Intensity：

      In `main.m`, locate the following variable:
        ```matlab
        light_intensities = [1.7, 4.8, 15.2, 39.4, 125, 444, 1406, 4630];
        ```
      You can modify the values in this array or add/remove elements to simulate the light intensities you are interested in (units: photons/μm²/s).

  Modifying Simulation Time & Flash Parameters:

      In `main.m`, locate the following variables:
        ```matlab
        t_total = 5.0; % s
        flash_start = 0.5; % s
        flash_duration = 0.02; % s
        ```
      `t_total`: The total simulation duration.
      `flash_start`: The onset time of the light flash.
      `flash_duration`: The duration of the light flash.

  Modifying Plotting Ranges:

      Open the `plot_final_results_figure2.m` file.
      The `plot_subplot` function calls within this file are responsible for drawing each subplot. Its fifth argument, `y_limits`, defines the y-axis range.
      For example, to modify the plotting range for `IKCa`, find the corresponding line:
        ```matlab
        % Original line:
        % plot_subplot(7, cellfun(@(x) x.IKCa, results, 'UniformOutput', false), '(pA/pF)', 'I_{KCa}', [-0.02, 0.22], 1/Cm);

        % To change the range to, for example, [-0.05, 0.25], modify it as follows:
        plot_subplot(7, cellfun(@(x) x.IKCa, results, 'UniformOutput', false), '(pA/pF)', 'I_{KCa}', [-0.05, 0.25], 1/Cm);
        ```

#### **5. File Descriptions**

  main.m`: **Main Script**. Sets up parameters, runs the simulation, and calls the plotting function.
  derivatives.m`: **Core ODE Function**. Defines the differential equations for all 94 state variables, which are called by the `ode15s` solver.
  init_parameters.m`: **Parameter Initialization**. Defines all physical and chemical constants used in the model.
  init_state_variables.m`: **State Variable Initialization**. Sets the dark-adapted initial values for all state variables.
  calculate_currents.m`: **Current Calculation Function**. Calculates all transmembrane ionic currents based on the current state of the model.
  phototransduction_cascade.m`: **Phototransduction Cascade Function**. Simulates the biochemical reactions from photon absorption to PDE activation.
  plot_final_results_figure2.m`: **Plotting Function**. Visualizes the simulation results.
  calculate_ATP_consumption_accurate.m`: **ATP Consumption Function**. Used for post-processing analysis of energy consumption.

<img width="865" height="584" alt="图片" src="https://github.com/user-attachments/assets/755f3dab-92f9-4696-b697-5cb15f99cb45" />
Fig.1 The network structure of the present model of phototransduction in a red cell, each abbreviation is explained and the kinetics of each reaction are elucidated). Irreversible reactions are marked with an arrow indicating the direction of each reaction, Colours are used to distinguish between active molecules (ie. in different yellow tones: molecules that carry on the amplification cascade in the following reaction), and those are necessary for signaling, but are cither inactive intermediats or specis devoted to shut-off regulatory mechanisms. A bold bonder marks the activated species involved in the amplification.

<img width="865" height="596" alt="图片" src="https://github.com/user-attachments/assets/fc185da3-40f3-4384-9a44-e6163a08b83f" />

Variable	English Name	Unit	Significance & Interpretation
Core Electrophysiology & Chemical Signals			
Vm	Membrane Potential	mV	The overall voltage state of the cell, representing the final output of all integrated ionic currents. It hyperpolarizes (becomes more negative) in response to light, which is how the cell signals to the brain.
[cGMP]	Cyclic GMP	µM	The key second messenger that links the light signal to the electrical signal. Its high concentration in darkness keeps the dark current channels (ICNG) open.
[Ca²⁺]os/is	Calcium Conc. (Outer/Inner Seg.)	µM	Another critical second messenger. Its concentration is affected by ion flow and, in turn, regulates the phototransduction cascade (light adaptation) and calcium-activated channels.
[K⁺]i	Intracellular Potassium Conc.	mM	Determines the electrochemical driving force for potassium currents, fundamental to maintaining cellular homeostasis.
[Na⁺]i	Intracellular Sodium Conc.	mM	Determines the electrochemical driving force for sodium currents, fundamental to maintaining cellular homeostasis.
[Cl⁻]i	Intracellular Chloride Conc.	mM	Determines the electrochemical driving force for chloride currents, important for stabilizing the membrane potential.
Major Ion Currents			
I_CNG	Cyclic Nucleotide-Gated Current	pA/pF	The core of the "dark current". Activated by cGMP in darkness, allowing Na⁺ and Ca²⁺ influx. Its closure is the most direct cause of the light response.
I_h	Hyperpolarization-Activated Current	pA/pF	An inward current that activates as the membrane becomes more negative. It counteracts excessive hyperpolarization and helps the membrane potential recover after a light flash.
I_Kv	Voltage-Gated Potassium Current	pA/pF	A primary outward current responsible for hyperpolarization. In the dark-adapted state, it is persistently active to balance the strong inward dark current.
I_CaL	L-Type Voltage-Gated Ca²⁺ Current	pA/pF	Located mainly at the synaptic terminal, it mediates Ca²⁺ influx during depolarization, which is necessary for neurotransmitter release.
I_NaK	Sodium-Potassium Pump Current	pA/pF	Consumes ATP to actively pump Na⁺ out of and K⁺ into the cell, maintaining the fundamental ion concentration gradients.
I_PMCA	Plasma Membrane Ca²⁺ Pump Current	pA/pF	Consumes ATP to actively pump Ca²⁺ out of the cell, maintaining a very low intracellular calcium concentration.
I_NCKX	Na⁺/Ca²⁺-K⁺ Exchanger Current	pA/pF	Located in the outer segment, it is the primary mechanism for extruding Ca²⁺ that enters via the dark current.
I_NCX	Na⁺/Ca²⁺ Exchanger Current	fA/pF	Located in the inner segment, also responsible for extruding Ca²⁺.
I_KCa	Ca²⁺-Activated K⁺ Current	pA/pF	Opens when intracellular Ca²⁺ rises, producing an outward current that helps to hyperpolarize the cell.
I_ClCa	Ca²⁺-Activated Cl⁻ Current	pA/pF	Opens when intracellular Ca²⁺ rises, generally serving to stabilize the membrane potential.
Slow Regulation Systems			
J_NKCC1	Na⁺-K⁺-Cl⁻ Cotransporter Flux	mM/s	A slow, long-term regulator responsible for transporting Cl⁻ into the cell to maintain homeostasis.
J_KCC2	K⁺-Cl⁻ Cotransporter Flux	mM/s	A slow, long-term regulator responsible for transporting Cl⁻ out of the cell to maintain homeostasis.
Leak Currents			
I_L,K	Potassium Leak Current	fA/pF	A small, constantly active outward current that is crucial for fine-tuning the resting membrane potential.
I_L,Na	Sodium Leak Current	fA/pF	A small, constantly active inward current that is crucial for fine-tuning the resting membrane potential.
I_L,Cl	Chloride Leak Current	fA/pF	A small, constantly active chloride current that is crucial for fine-tuning the resting membrane potential.
I_L,Caos	Outer Segment Ca²⁺ Leak Current	fA/pF	A small, constantly active calcium leak current in the outer segment.
I_L,Cais	Inner Segment Ca²⁺ Leak Current	fA/pF	A small, constantly active calcium leak current in the inner segment.

