---
layout: post
title: MISRA Enum Loop
excerpt: Use of enums for loops with MISRA rules
modified: 5/07/2021, 9:00:24
tags:
  - Qt
  - Cpp
comments: true
category: blog
---
This article discusses the utilization of enum types as loop indexes, highlighting their value in expressing the desired iteration type without conflating conceptual types. Enum types serve to enhance code readability and maintainability by avoiding cumbersome casts and mitigating quality assurance warnings. 

Consider a scenario where a system comprises several power supplies. During the system initialization process, it becomes essential to inspect the states of all power supplies. Opting for an enum type proves superior to macros for iteration purposes. Therefore, a power supply enum type is created to facilitate this process.

```
  /*Available power supplies.*/
  typedef struct
  {
    PS_NORMAL = 0u,
    PS_AUX_1,
    PS_AUX_2,
    PS_GEN,
    PS_MAX
}HS_PowerSupply_Type;

/*Initialize the power supplies independently of their state.*/
void PowerSupply_Init(void)
{
  /*Expressing HS_PowerSupply_Type PS_LB to iterate.*/
  uint8 index = 0u;
  for(index = 0u; index < (uint8) PS_MAX; index ++)
  {
    PowerSupply_Restart((HS_PowerSupply_Type) index);
  }
  /*…*/
}
```
Some of us got accustomed to use integer in for loop indexes; but it is also possible to use enum types as indexes as they are essentially integers.

At first glance, this code seems neat but it has a QAC warning regarding **MISRA 2012 Rule 10.5** which stands that the casting of uint8 to HS_PowerSupply_Type enum type is inappropriate in the PowerSupply_Restart function. This is because it is probable that you can set an integer value that is big enough that it cannot be expressed by the enum type.

We can try to solve this QAC warning by using HS_PowerSupply_Type enum type as loop index instead of the uint8.

```
/*Initialize the power supplies independently of their state.*/
void PowerSupply_Init(void)
{
  /*Initial HS_PowerSupply_Type value equal to 0.*/
  HS_PowerSupply_Type index = PS_NORMAL;
  for(index = PS_NORMAL; index < PS_MAX; index ++)
  {
    PowerSupply_Restart(index);
  }
  /*…*/
}
```
We erased the annoying casts but if we check QAC again, we will find another MISRA warning **MISRA 2012 Rule 10.**1: The HS_PowerSupply_Type enum type is an inappropriate operand for the operation ++. This is because an enum might have gaps between constants referring to values not possible to express. For example:

```
typedef enum
{
  VRM_NORM = 0u,
  /*There is no enum constant for 1u and 2u integer*/
  VRM_LOW = 3u,
  VROM_OVER,
  VRM_MAX
}VRM_State_Type;

/*10ms VRM main task.*/
void VRM_Task(void)
{
  /*Initial VRM_State_Type value equal to 0.*/
  VRM_State_Type index = VRM_NORM;
  for(index = VRM_NORM; index < VRM_MAX; index ++)
  {
    /*Ups No VRM definition to 1 and 2*/
    SetVRMState(index);
  }
  /*…*/
}
```
Hence, the best solution is to ensure that the enum is consecutive and that the initial enum constant is always the first one. The way to solve this is in the enum declaration a comment should be used to make other developers aware of these constraints. Furthermore, in the for loop, the respective justification should be placed regarding the enum consecutiveness and the first enum constant place keeping.

A final remark, if the index is used only as an array index then the uint8 approach is the ideal one:
```
void PWMDerate_Calculation(void)
{
  /*Expressing HS_PowerSupply_Type PS_NORMAL to iterate.*/
  uint8 index = 0u;
  for(index = 0u; index < PS_MAX; index ++)
  {
    /*Clean the last cutOff value before making calculations*/
    cutOffSignal = 0u;
  }
  /*…*/
}
```
