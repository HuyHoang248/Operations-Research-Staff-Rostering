# Operations-Research-Staff-Rostering

## 1. Project Background

![image](https://user-images.githubusercontent.com/92867270/202965125-6dea98f2-665e-495d-b355-7aa562790f8e.png)

The previous slide illustrates a typical staff roster, in which:
+ A Cell contains a Shift, assigned to a Staff in a Day, i.e, Staff A_SAC is assigned to Shift 2M on Jun 1st
+ There are some constraints to make a valid roster:

  + 2 types of shifts:
    + Non-Working shift, showing as O
    + Working shift, showing as
    + Morning shifts: 1M, 2M
    + Afternoon shifts: 1A, 2A
    + Night shifts: 1N, 2N

  + Staffs can only receive several Shifts due to their skillsets, i.e, Staff C_SAC can only receive shifts 1A, 2A, and 1M, 2M
  +  Maximum Total Working Hours that a staff can work. You can see the Actual Total Working Hours recorded in Staff Statistics on the right panel, i.e Staff A_SAC totally spends 177 Working Hours
  + Requirement on total number of staffs need to work Per Day, showing in Coverage Statistics at the bottom panel, i.e., we need At Least 1 staff for Afternoon shift (either 1A or 2A)
  + Shift Patterns: there are some repeated patterns, i.e, 1M→2M→ 1N→ 2N, 1N→ 2N→ O
  
  ## 2. Project Outlines
  
  1. Describe what outcome variables you need to predict
  2. Describe which features and their data format need to be used to train and predict the outcome variables. Please remember to capture the constraints to have a valid roster
  3. Implement A.I algorithms and their corresponding configuration for above features & outcome variables.
  
  ## 3. Project Approach
  
  1. Define variables:
  
    + Staff name: staff_name = ['A_SAC', 'B_SAC', 'C_SAC', 'D_SAC', 'A_AC1', 'A_AC2', 'A_AC3', 'B_AC1', 'B_AC2', 'B_AC3'] # 10 staff

    + Shifts_name = ['Morning', 'Afternoon', 'Night']  #0: Morning, 1: Afternoon, 2: Night

    + Days = ['Mon', 'Tue', 'Wed', 'Thur', 'Fri', 'Sat', 'Sun'] # 7 days in a week
  
  2. Use Google - OR tools and define the objectives and constraints
  
    + 2 types of shifts:

      + No-Working Shift, O
      + Working Shift:
          + Morning shifts: 1M, 2M

          + Afternoon shifts: 1A, 2A

          + Night shifts: 1N, 2N
    + Each shift is assgined to >=1 staff per day
    + Each Staff only <= 2 Consecutive Same Shifts
    + Staff C_SAC cannot receive Night Shift. Staff C_SAC: n = 2, Night Shift s != 2 for all day
    
    + Each staff must take one day off after 2 Night shift
    + Night shift takes value of 2 in our confit in shifts --> we can take 3 consecutive days the sum of shift must less than 4
    + It can interpreted as following:

      + Because s can take value 0 and shifts can take value of 0 so if shift [0,1] * s[0,1,2] = 0. I cannot identify whether shift = 0 or s = 0 (Morning).
      + Therefore, I need to multiply by (s + 1). Shift [0,1] * (s + 1) [1,2,3]
      + For each staff, three consecutive day, the shifts in number must <= 6
    + For the pattern 2 Morning Shift then 2 Night Shift
    + For 5 consecutive days, if a staff take 2 Morning Shift in consecutive then this staff should take 2 night shift for next 2 consecutive days and then one day Off
      + Because s can take value 0 and shifts can take value of 0
      + Therefore, I need to multiply by (s + 1). Shift [0,1] * (s + 1) [1,2,3]
      + For each staff, five consecutive day, the shifts in number must <= 8
    + What if, Shift [0,1] * (s + 1) [1,2,3] = 2 for 4 consecutive days then the sum  will be = 8. 
      + It means a staff can take 4 Afternoon Shift in 4 consecutive days. However we already defined constraints 
      + Each Staff only <= 2 Consecutive Same Shifts so we can eliminate the 4 Afternoon Shift Each staff
    (Please see the configuration in the code)
