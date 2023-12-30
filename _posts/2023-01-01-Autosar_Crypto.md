---
layout: post
title: Autosar Crypto
excerpt: Autosar Crypto
modified: 5/07/2021, 9:00:24
tags:
  - Qt
  - Cpp
comments: true
category: blog
---
# Autosar Crypto Stack
The crypto stack defines a standardized set of crypto services for applications of cybersecurity. Crypto services vary in functionality such as symmetrical encryption, asymmetrical signatures validation, and computation of hashes. The module that manages all these functions is called Crypto Service Manager (CSM), CSM is located on the service layer and it is able to abstract all the functionalities into jobs. A Job might contain different security keys, queues, and primitive types. Usually, the cryptographic services are executed on additional provisions (i.e. dedicated microcontroller core) due to the intensive computation that might trigger. Libraries of the crypto driver can be software-driven or be a hardware device.

# Cryptography Basics
Cryptography is the mechanism/algorithm collection to perform cybersecurity on a system.  Cryptography is divided into 2 types of cryptography:
1. **Symmetric cryptography** uses the same steps to encrypt and decrypt. Decrypt performs the encrypt steps but in reverse order. Symmetrical cryptography can be defined as either block cryptography or data flow cryptography.
2. **Asymmetric cryptograph**y uses different steps to encrypt and decrypt. Some examples of asymmetric cryptography are integrating factor, discrete logarithmic function, and elliptical curve.

Some other complementary cryptography mechanisms are:
1. **Hashes** are interface functions that hide the real values of data sets.
2. **Protocols** are complex mechanisms to use different types of cipher with hashes.

There is a principle called **Kerckhoffs Principle** which indicates that the security of a system relies almost entirely on the secrecy of the key and not on the secrecy of the algorithm.

A hash function takes one input value with variable length, then the hash maps the output with a predefined length to represent the fingerprint of data. A secure hash function is infeasible to the inverse computation of the output and it is infeasible to the case where two inputs cannot yield to the same mapped output. Hash functions perform huge changes to relative small input differences. Hash functions are not defined as cybersecurity mechanisms but are very important as a complement to some real security mechanisms.

## Symmetric cryptography
Symmetric cryptography uses the same key to encrypt and decrypt. This type of encryption is normally used for data confidentiality, integrity, and authenticity. The distribution of keys shall be careful because anyone with the key can access all security mechanisms.

The most used in the automotive industry symmetric cryptography is the Advanced Encryption Standard (AES), AES is based on block cryptography algorithms and needs the definition of the key and the data length (128 bits, 192 bits, or 256 bits). AES performs cycles of encryption rounds, these rounds operate combinations of byte substitution, shifting of bytes, or key addition to produce a CipherText.
In general, symmetric cryptography can create messages of authentication of code (MACs) during their procedure using the secret key. MAC is a fixed-side digital code that can be only created by the key creator. The MAC is transmitted along with the encrypted data, if the receiver can decipher the MAC, then it is assumed that the encrypted data can be decrypted too. There are two types of MACs, the one defined by hash functions (HMAC) or the one defined by block encryption (CMAC).

## Asymmetric cryptography
Asymmetric cryptography uses two keys (one public and another private). These keys have relatively large lengths. The public key can be shared with all the entities that can collaborate with the system; whereas the private key cannot be shared with all the entities of the system and shall be dedicated to a specific entity.

Asymmetric cryptography algorithms are based on one key encrypting the data and the other one decrypts the data. For example, if the public key is used to encrypt data, then the private key is used to decrypt the data. The most used algorithms of asymmetric cryptography are RSA(Rivest, Shamir, and Adleman) and ECC (Elliptic Curve Cryptography).

### Digital Signature.
The digital signature is based on asymmetric cryptography and offers data authenticity and data integrity. The digital signature creator generates the signature by the hash generation of complex data, to later encrypt this data before transmitting it to the user. The user decrypts the received data using a private key; after that compare the hash data with the decrypted hash data and if they are the same, then the data integrity and data authenticity are guaranteed.

Digital signatures are used by certifications. Certifications are mechanisms to ensure that one data-sending entity is allowed to send those data. Certifications usually contain personal data from the sender (for example IDs, sensitive information), this personal data has an expiration time to use. One use case from the automotive industry is the tester identification with certifications to allow access to the ECU. This ECU access is supported by different certifications for each access level (Root, Tester, Checker, …).

# Detailed Crypto Stack
The EE architecture with a cybersecurity mechanism can involve multiple layers of security from the ECU perspective.
1. The first layer is the vehicle access point layer that can be secured by adding firewalls and intrusion detection.
2. The second layer is the in-vehicle network interfaces that can be secured by adding firewalls, key managers, filtering routing, or delimitating interface zones.
3. The third layer is the public inner-vehicle network that can be secured principally by message authentication protections.
4. The fourth layer is the private inner-vehicle network that can be secured principally by message authentication protections.
5. The fifth layer is the ECU by itself that can be secured principally by anti-tampering security mechanisms.

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/1a02a9be-d693-4e63-8dca-4655b66f32c4)
 
Systems (more usual to legacy systems) encounter major blocks while trying to integrate cybersecurity features. These major blocks can be the need to change the architecture of the system and still sometimes the microcontroller chip, additionally, there might be a new requirement to support MACs or message security wrappers that require higher bandwidth. Some approaches that permit overcoming these blocking areas and support cyber-security are the decouplings of the network communication, use standardized concepts, or change the vehicle communication protocol. For example, moving a legacy bus payload of CAN (8 bytes) to a higher bandwidth network such as CAN-FD (64 bytes) into a dedicated bus allows the insolation of sensible information, and the high bandwidth does not affect other legacy networks.

## Autosar Crypto Stack

The crypto stack allows synchronous job processing or asynchronous job processing depending on the use case: 
1. Synchronous jobs are used when the services are required to be calculated fast and the overhead from the callback use is not acceptable; for example fast generation of MACs on the SecOC (Secure Onboard Communication) module.
2. Asynchronous jobs are used when the service lasts a long time and the synchronous calls would affect the other task calls; for example, the signature verification requires relatively large execution times.
   
The cryptographic jobs shall permit the static configuration of keys. Regardless of the type of crypto algorithm, every key is referenced by a unique ID, this ID permits the addressing of keys to users without giving a direct interface to extract the encrypted data.

The crypto stack shall identify all the crypto functions as an abstract request to the crypto drivers. This means that the crypto stack would identify the random generation number, encryption/decryption functions, MAC generation, signature functions, and hash calculation as an abstract crypto primitive that can be requested by homogenous interfaces on the specific crypto drivers. Moreover, the crypto management that is part of the crypto stack shall be able to support abstraction of interface for specific key functions such as the derivation of symmetric keys, key exchanging, key wrapping, or key extraction.

Crypto stack can support interfaces for additional functions such as the parsing of certification, detection of invalid keys, and secure counter interfacing. Crypto manager might request the read and increment of the counter from the crypto drivers using services interfaces to avoid the direct access of the users to these counters type. The crypto manager parses the incoming certifications to extract the keys, and can also detect the invalid keys to cover corner cases of some crypto algorithms (mainly RSA and ECC) that cannot support this verification and rejection of keys; this detection is commanded to be performed on the crypto driver itself.

The Autosar cryptography stack is composed of the following modules:
1. **Crypto Service Manager** (CSM): This module communicates with the SWCs to offer services and handlers to the cryptography operations:
  1. Random number and hashing generation.
  2. Data encryption/decrypt.
  3. MACs.
  4. Digital Signature.
  5. Models of operations based on “jobs”.
  6. Asynchronous/Synchronous process of jobs.
  7. Generation, derivation, and exchange of keys.
  8. Freshness counting
2. **Crypto Interface** (CRYIF): This module enables CSM to access Crypto Driver features based on standardized APIs.
3. **Crypto Driver** (CryptoSW and CryptoHw): These modules are the ones performing the cryptographic functions. CryptoSW uses SW libraries whereas CryptoHW uses HW capabilities (SHE, HSM, TPM, …).

![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/726b19a5-6096-4172-985b-c8c7af56a3d3)

### Crypto Service Manager
Crypto Service Manager (**CSM**) is the service module from BSW that manages the crypto services to secure data and processes. A **CSM Job** is the abstraction of CSM keys, CSM queues, and CSM primitive types. CSM has the properties to be able to dispatch jobs based on priorities. The jobs can be synchronous or asynchronous, Synch jobs wait for the returning value of a finished call and do not use callbacks; whereas async jobs are inserted into the queue based on their priority when the respective callback is finished.

Based on the exchange of information on the security stack, each CSM key is mapped to a CRYIF key and a Crypto Driver key.

CSM Queues are mapped based on the Crypto Driver, CRYIF uses one channel for each CSM Queue to a Crypto Driver in order to process one job at a time.

The crypto stack communication is as follows:
1. One SWC asks for a CSM service using the API Csm_Encrypt(). This API use Job IDs to identify every encryption process.
2. CSM adds the required job to one of the available CSM queues, the scheduler processes this job based on its priority.
3. Then CSM executes Csm_MainFunction() to transmit the selected job to lower modules.
4. The job is transmitted to the Crypto Driver, which processes it, and returns a specific value to let CSM know the result.
5. CSM returns the result to the SWC while removing the processed job from the queue.

CSM primitives define each job type based on:
1. Crypto predefined generic algorithms.
2. MAC generation or verification.
3. A digital signature generation or verification.
4. Encryption/Decryption of symmetric keys.
5. Encryption/Decryption of asymmetric keys.
6. Hashing, random number generation, etc.
7. Specific algorithm configuration
8. Algorithm family (AES, RDS, …)
9. Algorithm mode.
10. Job processing mode.
11. Length of input/output data.

### Security on On-board Communication
Security on On-board Communication (**SecOC**) is an additional module at the  Autosar communication stack related to PDU transmission authentication and integrity. SecOC retrieves PDUs from PDUR and adds security information to encrypt based on the PDU payload and the SOME/IP transformer use. SecOC supports various communication buses such as CAN, FlexRay, and Ethernet but is not supporting LIN yet. 

![03](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/804e552e-506a-4955-a2e5-0d5164bc2537)

The principal interaction flow of SecOC is:
1. PDUR receives one PDU from one specific channel (CAN, FlexRay, Ethernet).
2. PDUR transmits the PDU to SecOC, then SecOC converts this PDU to a “secure” PDU.
3. SecOC along with CSM verifies the PDU authenticity and integrity.
4. SecOC returns the PDU without security wrappers to PDUR to finish the PDU processing.
5. When SecOC determines that one PDU is insecure, SecOC discards the PDU, and PDUR receives nothing from SecOC.
6. SecOC communicates the PDU verification results to ASW.
7. Additionally, SecOC can support PDU data freshness.
   
![04](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/7e92b58c-1f0c-4a13-ac0e-6039abaa8982)

## Crypto Interface
Crypto Interface (**CryIf**), located in the ECU abstraction layer, is the implementation of cryptography abstraction made by the crypto drivers and coordinated by CSM access. CryIf offers independent APIs from HW/SW crypto drivers to superior modules to process the crypto operations. 

## Crypto Driver
Crypto Drivers, located in the microcontroller abstraction layer, are the principal responsibilities of the crypto operations. They specify the crypto capabilities (key storage and algorithm support) and the composition of the key. Each Crypto Driver is represented by a crypto driver object, each crypto driver object depends on a vendor-defined initialization. 

The key component is the classification of key types made by the Crypto Drivers. Examples of these key types are Private key, Public key, hash values, random number certification, etc. Autosar defines these key types to perform crypto operations. Each composition key type has one ID, Autosar defines a key type ID below 1000, when two different key types have the same ID, then these key types can be used such the same type (Length, operations, access, etc). Keys ID that is above 1000 is free-definition to e used by the vendor.

Crypto driver can only dispatch one CSM job at the type, the pre-configuration determines the capabilities of each crypto driver object to support the kind of jobs that CSM shall acknowledge. Crypto driver support state of IDLE, START and FINISH to denote its state to upper layer modules and therefore avoid misled job processing (for example, CSM sending jobs when the Crypto Driver is not ready yet). Every synchronous or asynchronous job processing is called by CryIf when the job had been finished by the Crypto Driver.

