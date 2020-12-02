
Principles and problems of pdp storage proof mechanism


1) Principle of pdp mechanism PDP
It is currently the most widely used and most mature storage integrity certification mechanism in the distributed storage field. It can quickly determine whether the data on the remote node is complete, and is mainly used to detect the integrity of large data files. An application scenario of PDP mechanism

as follows: 
1. Alice asks Bob to memorize a set of information;
2. Alice herself will not write down this group of information;
3. Alice asks Chris to confirm whether Bob still remembers this group of information;
4. Chris does not understand the content of this set of information.
We replace these roles: Alice is the user, Bob is the storage absenteeism, and Chris is the third-party reviewer (hereinafter referred to as TPA).


It is generally divided into two steps:

1.setup stage:
 Initialization phase. First, the user runs the key generation algorithm to generate a key pair (pk, sk); then, divides the stored file into blocks F=(m1, m2,..., mn); after that, runs the data block label generation algorithm as the file Each data block generates a homomorphic label set Φ; finally, the data file F and the signature set Φ are stored in the cloud at the same time, and the local {F, Φ} is deleted.

2. Challenge stage:
Verify the request phase. The user or TPA acts as the verifier and periodically initiates integrity verification. Randomly pick c block indexes {s1, s2,...,sc} from the file F block index set [1, n], and select a random number vi for each index si, and combine the two to generate a challenge request Chal sent to the server. The server as the prover, according to the data file {F,Φ} stored on its server, invokes the evidence generation algorithm to generate the integrity proof P, and returns it to the verifier. After the verifier accepts the evidence, it executes the evidence detection algorithm to verify whether the evidence is correct.


The existing PDP mechanisms include:

1. Based on the MAC authentication code PDP mechanism, MACPDP, before the user saves the file, the user selects a random value, and then the user adds the random value to the entire file and calls the hash function to obtain the MAC value, and sends the verification metadata to the TPA; During verification, TPA sends a challenge request to the storage server, and the remote server calculates the value of the file and returns it to TPA. After comparing the value stored locally, TPA determines whether the data file is complete. This algorithm is high-performance, but Can only be verified a limited number of times​.

2. PDP mechanism based on RSA signature, RSA-PDP, using the homomorphism of rsa signature mechanism, can be verified countless times, but the amount of calculation is large, the storage and communication volume is also large, it is not suitable for large-scale storage, and does not support dynamic Operation​).


3. The PDP mechanism based on BLS signature, SE-PDP, reduces the amount of storage and communication compared to rsa, but the amount of calculation is still large​.


4. Support dynamic operation PDP mechanism (D-PDP), etc. The above, such as PDP_MACPDP and PDP_SEPDP, are integrated in the libpdp library of CVT. The current implementation of CVT uses a mechanism similar to MACPDP because CVT needs to prioritize performance, and there is currently no need for multiple verifications and dynamic operations.



2) Problems with pdp mechanism ​

1. The storage integrity certification cannot guarantee the data integrity at all times.
TPA nodes are mostly fully centralized nodes, and the decentralization of central nodes cannot be guaranteed.