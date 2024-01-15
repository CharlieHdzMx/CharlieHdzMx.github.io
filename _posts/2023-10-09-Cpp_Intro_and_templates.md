---
layout: post
title: C++ Pointers
excerpt: Article about C++ Pointer main features
modified: 5/07/2021, 9:00:24
tags:
  - Cpp
comments: true
category: blog
---

**Pointers**, akin to Java references, are fundamental language mechanisms used to reference memory. Primarily, they enable the efficient passing of potentially large data sets. However, the improper implementation of pointers poses a significant risk of memory corruption. As a precaution, certain implementations may benefit from using **STL** containers instead of pointers. A notable advantage of STL containers is their built-in mechanisms for enhanced control over object addressing, thereby mitigating the risk of memory corruption.

Foremost among its benefits, using pointers allows us to reference the address of a specific object rather than passing the entire object, which can be notably resource-intensive.

The primary function of a pointer revolves around indirection, facilitated by the dereferencing unary prefix operator **(*)** that enables us to reference the object pointed to by the pointer. In addition to indirection, the address-of operator **(&)** serves to directly obtain the address of a particular object. To exemplify, the following snippet illustrates the usage of both the indirection and address-of operators:

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/1de0d43d-3cbf-4fdb-bb7e-a44db39cd390)

# void* Pointer
In low-level programming, the **void*** pointer is employed to handle variables of unknown types. It's important to highlight that this type of pointer cannot serve as a function argument or point to a class member, leading to potential compile-time errors during certain operations like dereferencing and increment/decrement. Notably, void* can be explicitly converted to another type as needed. To exemplify, consider a function attempting to use the dereference and increment operators with a void* pointer, followed by explicit conversion back to an **int***:

![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/e1fbbc8c-6c84-4394-8f0c-3c18bad3dbf2)

# nullptr Literal
When there is no need for a pointer to reference an object, it is advisable to use the _**nullptr**_ literal. This practice enhances code readability and minimizes potential confusion in pointer implementations. Prior to the introduction of the nullptr literal, zero (0) and NULL (used in C) were employed as notations for the null pointer, often leading to misunderstandings. It is crucial to note that this literal should be exclusively used with pointers.

# Const Pointers
In C++, the term "constant" encompasses two interconnected concepts:

1. _**constexpr**_, which evaluates and guarantees constness at compile time.
2. _**const**_, which establishes the immutability of variables within a specific scope.

Employing const pointers proves beneficial when we aim to prevent any modification of an object pointed to, particularly when used as a function argument. For instance, the following code demonstrates four ways to utilize const with pointers:

![03](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/812db73e-cb0b-420a-8a79-d44088bd0d78)

# References Alias
A **reference** serves as an alias for an object, differing from pointers in that it imposes no performance overhead and shares the same syntax as the name of its associated object.

References are primarily employed to define efficient parameters and return values for functions. This proves especially useful when dealing with substantial objects that are costly to pass by value to a function, as a reference parameter is initialized by the actual argument passed during the function call, thereby avoiding the need to duplicate the object itself.

An improved alternative to a reference parameter is a reference to a const parameter. This instructs the compiler to ensure that the function does not attempt to modify the object, potentially saving considerable debugging time and stress.

As an illustration, consider a function declaration using a reference to a const in a cryptographic program. This function alters the value of each character in a string based on a special key:

![04](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/a0b5a1b3-5a5b-464b-b079-e702f4ae21b8)

# EXTRA Rvalue References
With the purpose of encourage your curiosity... What's the most effective swap function?
![05](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/6de7e8e8-a6c7-4ad5-8b10-26976b9dee49)

```
double Fraction::toDouble(){
 
  return 1.0*(m_nNumerator/m_nDenominator);
}

//Don't you remember the const reference used as function parameter?﻿
//Check past articles﻿
//Two fraction addition﻿
Fraction& Fraction::add(const Fraction & other){

  Fraction tempFraction;
  if(m_nDenominator==other.m_nDenominator){
 
    tempFraction.m_nNumerator=m_nNumerator+other.m_nNumerator;
    tempFraction.m_nDenominator=m_nDenominator;
  }
 
  else{  
    tempFraction.m_nNumerator=(m_nNumerator*other.m_nDenominator)+(m_nDenominator*other.m_nNumerator);
    tempFraction.m_nDenominator=m_nDenominator*other.m_nDenominator;
  }
 
  return simplify(tempFraction);
}

//We wish to change f inside the function-so isn't a const reference
//in constrast than the last function﻿
//This function is ﻿awesome!!
﻿
Fraction& Fraction::simplify(Fraction& f){

//They may be static member also... but we're using them only here. 
  const int sizeArray=46;
  const int aPrimes[]={ 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41,
                        43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97,
                        101, 103, 107, 109, 113, 127, 131, 137, 139, 149,
                        151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199 };
 
 
  for(int i=0; i< sizeArray;){
    //If we found a number which is the divisible for numerator and denominator
    if((f.numerator()%aPrimes[i]==0)&&(f.denominator()%aPrimes[i]==0))
      f.set(f.numerator()/aPrimes[i],f.denominator()/aPrimes[i]);
    
    else
      ++i;
  }
  return f;
}

//End
```

