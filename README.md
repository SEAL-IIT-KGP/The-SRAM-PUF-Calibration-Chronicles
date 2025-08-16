# ML-Assisted Reliable SRAM-PUF

This repository contains the code and data for the paper:  
**"Enhancing SRAM-Based PUF Reliability Through Machine Learning-Aided Calibration Techniques."**

⚠️ **Note:**  
- The raw data for Arduino **UNO** boards are included in this repo.  
- The raw data for **Zero** boards (CSV >100MB) will be provided via a Google Drive link in the camera-ready version.  
- Full code and datasets to reproduce all plots will also be included in the camera-ready release.  

---

## Repository Overview

The repository consists of the following components:

1. **Raw Data for Arduino UNO**  
2. **ML-Based PUF Recalibration (Continual & Transfer Learning)**  
3. **ECC Implementation with Soft Decoding**  
4. **Min-Entropy Loss Computation**

---

## 1. Raw Data for Arduino UNO

The folder `Raw_Data_UNO.zip` contains SRAM-PUF measurements for Arduino UNO boards under different temperature and voltage conditions.  

After extraction, you will find:  
- `RoomTemp_UNO_10Boards_Temp`  
- `RoomTemp_UNO_8Boards_Volt`  
- `CollectedDataAcrossVoltage_UNO_8Boards`  
- `CollectedDataAcrossTemperature_UNO_10Boards`  

Use `Response_processing.py` to read and process the raw `.csv` files.

---

## 2. ML-Based PUF Recalibration

This section contains **machine learning–based recalibration** methods for improving PUF reliability under environmental variations.

- Subfolders:  
  - `Temperature-UNO`  
  - `Temperature-Zero`  
  - `Voltage-UNO`  
  - `Voltage-Zero`

Each subfolder includes:  
- **Data files** (from Raw_Data_UNO.zip or Zero dataset)  
- **ML-based calibration scripts** for Continual and Transfer Learning  

### Running the scripts
- Continual Learning:  
  ```bash
  python MLbased_Recalibration_XX_Continual_Learning_YY.py
  ```
- Transfer Learning:  
  ```bash
  python MLbased_Recalibration_XX_Transfer_Learning_YY.py
  ```

Where:  
- `XX ∈ {UNO, Zero}`  
- `YY ∈ {Temp, Volt}`  

### Input dependencies
- Reliability Information (ambient):  
  - UNO: `Rel_UNO_AmbientTemp_nMeas15.npy`, `Rel_UNO_AmbientVolt_nMeas15.npy`  
  - Zero: `Rel_temp_Zero_ambient_Oct14.npy`, `Rel_volt_Zero_ambient_Oct11.npy`  
- PUF Golden responses:  
  - UNO: `GResp_UNO_temp_all.npy`, `GResp_UNO_Volt_all.npy`  
  - Zero: `GResp_temp_Zero_all_Oct14.npy`, `GResp_volt_Zero_all_Oct11.npy`  

### Outputs
- Corrected PUF responses:  
  - `CorrectedResp_XX_TransferLearning_YY.npy`  
  - `CorrectedResp_XX_ContinualLearning_YY.npy`

---

## 3. ECC Implementation

Located in the `ECC/` folder with two subdirectories:  
- `Soft-Decoding-UNO`  
- `Soft-Decoding-Zero`  

### Algorithms Implemented
- **Soft decision helper data algorithm** from Maes et al., CHES 2009  
- **Concatenated code**:  
  - Repetition (3,1,3) + RM(2,5) for UNO  
  - Repetition (3,1,3) + RM(2,4) for Zero  

### Running the scripts
```bash
python ECC/Soft-Decoding-UNO/Concat_code_softDecoding_ECC_withSRAMPUF.py
python ECC/Soft-Decoding-Zero/Concat_code_softDecoding_ECC_withSRAMPUF.py
```

### Input dependencies
- Reliability information (ambient)  
- Corrected responses from Transfer/Continual Learning  
- Golden responses at all temperatures/voltages  

### Code parameters
- Key width = 128 bits  
- Raw PUF bits required:  
  - 768 for UNO  
  - 1056 for Zero  

---

## 4. Min-Entropy Loss Computation

The `Min-Entropy-Loss/` folder implements empirical min-entropy loss analysis following Delvaux et al., CHES 2016.  

- BCH[15,11,1] standard array generated from all codewords  
- Dependencies:  
  - `Codeword_w.json` (BCH codewords)  
  - `std_array_p_complete.json` (standard array)  

### Scripts
1. Fixed helper data:  
   ```bash
   python entropy_BCH_15_11_1_fixed_helper_data_nolearning.py
   ```
2. Fixed helper data + Transfer Learning:  
   ```bash
   python entropy_BCH_15_11_1_fixed_helper_data_w_TL.py
   ```
3. Multiple helper data:  
   ```bash
   python entropy_BCH_15_11_1_multiple_helper_data.py
   ```

### Plotting results
To visualize uniformity, entropy, and loss across temperatures:
```bash
python Plot_all.py
```

---

## Citation

If you use this code or data in your research, please cite our paper:  

```bibtex
@article{pratihar2024enhancing,
  title={Enhancing SRAM-Based PUF Reliability Through Machine Learning-Aided Calibration Techniques},
  author={Pratihar, Kuheli and Chatterjee, Soumi and Chakraborty, Rajat Subhra and Mukhopadhyay, Debdeep},
  journal={IEEE Transactions on Computer-Aided Design of Integrated Circuits and Systems},
  volume={43},
  number={11},
  pages={3491--3502},
  year={2024},
  publisher={IEEE}
}
```
