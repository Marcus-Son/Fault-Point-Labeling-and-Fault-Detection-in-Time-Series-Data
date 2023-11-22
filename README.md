# Fault-Point-Labeling-and-Fault-Detection-in-Time-Series-Data

## Problem Definition
1. Background
   1. Heat treatment is one of the six core industries in the root industry, and it is essential in changing and improving the characteristics of metal materials. It is a process-dependent technology industry: each process, such as annealing, tempering, and quenching, uses heat, so energy consumption is considerable. Therefore, many major processes are carried out during the night when electricity costs are low. [1]
   2. In particular, many companies operate their facilities 24 hours a day to reduce energy loss when restarting heat treatment equipment. Moreover, heat treatment processes involve gas, high temperatures, and night shifts, making labor conditions more challenging compared to other industries. These conditions hinder the overall vitality of the industry due to a lack of new recruits and high turnover rates. [1]
   3. To address these issues, the heat treatment industry is also adopting smart factory solutions using information technology. [1]
   4. Real-time monitoring and control are becoming increasingly important in modern heat treatment processes, and various methods and technologies support this. [2]

2. Problem to Solve and Objectives
   2.1. Problems with heat treatment processes (related to defects)
        1. Real-time anomaly detection is not possible.
           a. In traditional heat treatment production lines, there is no system in place to monitor product quality in real-time during the production process. Therefore, it is common to take post-production measures after defective products have been produced. [3]
           b. Various measuring equipment operates in an analog manner, and production process workers can only visually inspect them, making it difficult to control the quality of the actual products produced. [3]
        2. Issues related to assignment numbers (job order numbers) in real-world situations
           - Heat treatment processes generally include three processes: heating, preservation, and cooling, and sometimes there are only two processes: heating and cooling. These processes are interconnected and do not stop. [4]
           - Even if the same process and equipment are used, there are situations where the distribution of data is different.
        3. It is impossible to confirm defects in the middle of the process.
           - Since heat treatment process conditions are visually confirmed by the production process workers, precise quality control of the produced products is difficult. If defects occur after product shipment, it is impossible to trace which process the defects occurred in, leading to productivity decline and increased production costs. [3]

   
