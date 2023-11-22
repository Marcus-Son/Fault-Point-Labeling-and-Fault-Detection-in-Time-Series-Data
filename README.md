# Fault-Point-Labeling-and-Fault-Detection-in-Time-Series-Data

## Problem Definition


1. Background

    1. Heat treatment is one of the six core industries in the root industry, and it is essential in changing and improving the characteristics of metal materials. It is a process-dependent technology industry: each process, such as annealing, tempering, and quenching, uses heat, so energy consumption is considerable. Therefore, many major processes are carried out during the night when electricity costs are low. [1]
    2. In particular, many companies operate their facilities 24 hours a day to reduce energy loss when restarting heat treatment equipment. Moreover, heat treatment processes involve gas, high temperatures, and night shifts, making labor conditions more challenging compared to other industries. These conditions hinder the overall vitality of the industry due to a lack of new recruits and high turnover rates. [1]
    3. To address these issues, the heat treatment industry is also adopting smart factory solutions using information technology. [1]
    4. Real-time monitoring and control are becoming increasingly important in modern heat treatment processes, and various methods and technologies support this. [2]
  


2. Problem to Solve and Objectives
    1. Problems with heat treatment processes (related to defects)
        1. Real-time anomaly detection is not possible
            1. In traditional heat treatment production lines, there is no system in place to monitor product quality in real-time during the production process. Therefore, it is common to take post-production measures after defective products have been produced. [3]
            2. Various measuring equipment operates in an analog manner, and production process workers can only visually inspect them, making it difficult to control the quality of the actual products produced. [3]
        2. Issues related to assignment numbers (job order numbers) in real-world situations
            - Heat treatment processes generally include three processes: heating, preservation, and cooling, and sometimes there are only two processes: heating and cooling. These processes are interconnected and do not stop. [4]
            - Even if the same process and equipment are used, there are situations where the distribution of data is different.
        3. It is impossible to confirm defects in the middle of the process.
            - Since heat treatment process conditions are visually confirmed by the production process workers, precise quality control of the produced products is difficult. If defects occur after product shipment, it is impossible to trace which process the defects occurred in, leading to productivity decline and increased production costs. [3]
    2. Objectives
        1. Propose a method to extract defect rates for each assignment number from existing collected data and label them.
        2. Develop a model using these labels that can trace the cause of defects at the point of occurrence, thereby improving quality control and reducing production costs.
        3. Considering the situation where the distribution of data collected for assignment numbers in the heat treatment pickling process may differ, we aim to create a generalized model that learns from the labels provided in this project.
        4. Utilize the results of the trained model in explainable artificial intelligence to provide a reference point for finding the cause at the defect point.
      


## 2. Definition and Data Processing of Manufacturing Data

1. Definition of Manufacturing Data Specification, Attributes, and Types
    1. Introduction to Manufacturing Data
        1. data.csv: Process data collected at 1-second intervals by assignment number
            1. Dataset Structure: Tabular format
            2. Data Shape: (2939722, 21)
            3. Key Variable Definitions and Types
                1. TAG_MIN(datetime) : 데이터가 수집된 시간
                2. 배정번호(int) : 공정의 작업 지시 번호
                3. 건조 1존~2존 OP(float) : 각 건조 온도를 유지하기 위한 출력량 (Output Percentage(%))
                4. 건조로 온도 1~2 Zone(float) : 각 건조로 Zone의 온도 값
                5. 세정기(float) : 세정기 온도 값
                6. 소입1~4존 OP(float) : 각 소입존 온도를 유지하기 위한 출력량 (Output Percentage(%))
                7. 소입로 CP 값(float) : 침탄 가스의 침탄 능력량(%) Carbon Potention Value
                8. 소입로 CP 모니터 값(float) : 모니터에 출력된 침탄 능력량???
                9. 소입로 온도 1~4 Zone(float) : 각 소입로 Zone의 온도 값
                10. 솔트 컨베이어 온도 1~2 Zone(float) : 각 솔트 컨베이어 Zone의 온도 값
                11. 솔트조 온도 1~2 Zone(float) : 각 솔트조 Zone의 온도 값
          2. quality.csv: Heat treatment quality data by assignment number
              1. Dataset Structure: Tabular format
              2. Data Shape: (136, 7)
              3. Key Variable Definitions and Types
                  1. 배정번호(int) : 공정의 작업 지시 번호
                  2. 작업일(datetime) : 공정 작업 날짜
                  3. 공정명(str) : 공정 이름
                  4. 설비명(str) : 설비 이름
                  5. 양품수량(int) : 양품 생산 수량
                  6. 불량수량(int) : 불량 생산 수량
                  7. 총수량(int) : 전체 제품 생산 수량
        3. train.csv : Summary statistics and label information of heat treatment process data
              1. Dataset Structure: Tabular format
              2. Data Shape: (2939722, 21)
              3. Key Variable Definitions and Types
                  1. 건조 1존~2존 OP_Avg, 건조 1존~2존 OP_Std (float) : 각 건조 온도를 유지하기 위한 출력량(Output Percentage(%))에 대한 평균과 표준편차 
                  2. 건조로 온도 1~2 Zone_Avg, 건조로 온도 1~2 Zone_Std (float) : 각 건조로 Zone의 온도 값에 대한 평균과 표준편차
                  3. 세정기_Avg, 세정기_Std (float) : 세정기 온도 값에 대한 평균과 표준편차
                  4. 소입1~4존 OP_Avg, 소입1~4존 OP_Std (float): 각 소입존 온도를 유지하기 위한 출력량 (Output Percentage(%))에 대한 평균과 표준편차
                  5. 소입로 CP 값_Avg, 소입로 CP 값_Std (float) : 침탄 가스의 침탄 능력량(%) Carbon Potention Value에 대한 평균과 표준편차
                  6. 소입로 온도 1~4 Zone_Avg, 소입로 온도 1~4 Zone_Std (float) : 각 소입로 Zone의 온도 값에 대한 평균과 표준편차
                  7. 솔트 컨베이어 온도 1~2 Zone_Avg, 솔트 컨베이어 온도 1~2 Zone_Std (float) : 각 솔트 컨베이어 Zone의 온도 값에 대한 표준과 표준편차
                  8. 솔트조 온도 1~2 Zone_Avg, 솔트조 온도 1~2 Zone_Std (float) : 각 솔트조 Zone의 온도 값에 대한 평균과 표준편차
                
2.Data Preprocessing
  1. data.csv
      1. EDA
          1. Handling Missing Values
              1. Check for missing values in each column of data.csv
                  - It was observed that the "Inlet 1 Zone OP" column has the most missing values.
                  - A total of 6043 missing values were identified.
              2. Handle missing values based on assignment numbers (Linear Interpolation)
                  - Reason for using Linear Interpolation: To maintain the time series information by assignment number, linear relationships are utilized to maintain a smooth flow of data.
          2. Visualization
              1. Histogram: Examine the distribution of each variable
                 ![image](https://github.com/jeewonkimm2/Fault_Point_Labeling_and_Fault_Detection_in_Time_Series_Data/assets/108987773/120f90d2-523e-4d84-bbd2-69a7f7b994bc)
              2. Box Plot: Check important statistical information such as median, quartile range, and outliers
                 ![image](https://github.com/jeewonkimm2/Fault_Point_Labeling_and_Fault_Detection_in_Time_Series_Data/assets/108987773/b8211ab9-1279-4da9-aa82-f8cc9afd2d3b)
              3. Line Graph: Observe the trends of variables by assignment number
                 ![image](https://github.com/jeewonkimm2/Fault_Point_Labeling_and_Fault_Detection_in_Time_Series_Data/assets/108987773/9eeddcd9-fbbe-41f4-9068-f80a4993a74a)
              4. Correlation between variables
                 ![image](https://github.com/jeewonkimm2/Fault_Point_Labeling_and_Fault_Detection_in_Time_Series_Data/assets/108987773/4585a396-f213-47b4-8733-75b538041f43)
          3. Data Preprocessing
   
