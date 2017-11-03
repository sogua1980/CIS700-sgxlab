Module 2: Side-channel Vulnerability and Protection
---

#### Preparation

1. C/C++ programming
2. Linux shell and text editing (e.g. vim or emacs)
3. SGX SDK
4. Sorting Algorithms 
 
#### SGX Emulator (1)

1. Install VirtualBox on your local OS.
2. Download the prebuilt VirtualBox image: [Google Drive](https://goo.gl/8sFN2y). The login username and password are both `sgx`.
    - Note our emulator currently only supports the setting of *64-bit `Ubuntu 14.04.3 LTS` guest OS in VirtualBox*.
    - Make sure the right Linux version by `uname -r`: `3.19.0-25-generic`
3. Load the emulator
```bash
sudo insmod ~/sgx-emulator/sgx.ko
lsmod | grep sgx #help you check whether module insert successfully
```
4. Run sample code
```bash
cd ~/sgx-emulator/sgx.ko
make #compile user code
make r #run user code
```
5. Unload the emulator
```bash
sudo rmmod sgx
```

#### Download Lab2

You can try to use shell command to clone lab repository from GitHub.
```bash
git clone https://github.com/syracuse-fullstacksecurity/CIS700-sgxlab.git
```
or you may go to [https://github.com/syracuse-fullstacksecurity/CIS700-sgxlab](https://github.com/syracuse-fullstacksecurity/CIS700-sgxlab), and download the repository.

After you downloaded the lab2 folder, you can start to finish lab tasks. Each time before compiling your code, you need to **load the emulator first**. Load procedure mentioned above, _3. Load the emulator_ .

When you confirm emulator loaded, you can use command below to compile and run your code.
```bash
cd [your lab directory] #you need to navigate to the lab folder
make #compile client code
make r #run client code
```

#### In-class Lab (2): Leakage of Sorting Algorithms

This lab requires you to complete three sorting algorithms for an array, and observe the data accessing and index referencing differences. The objective of this lab is to help you understand Side-Channel Vulnerability exposed doing sorting.

For your convinience, index accessing trace will be automatically printed out when you try to use a Class called `List`. You can use the class as the similar usage with array. For example,

```C++
List list;
list[0] = 3;
list[1] = list[2];
// Indices trace will be printed out
// (0)(2)(1)


List * lptr;
(*lptr)[0] = 0;
(*lptr)[2] = (*lptr)[1];
// Indices trace will be printed out
// (0)(1)(2)

```

##### Bubble sort/merge sort with access history of indices

1. Task 1 is about implementing your merge sort by filling `merge()` and `divide()`, and observing dependent(independent) indices access trace. Try to input multiple sets of array to see the difference.

2. Task 2 is about implementing your bubble sort, and observing dependent(independent) indices access trace. Try to input different sets of array, observe the difference
   * Expected output:
        - Assume input are 4 numbers, {10,30,20,50}, and your algorithm should print out the indices access trace: 
            `(0) (1) (1) (2) (1) (1) (2) (2) (2) (3) (0) (1) (1) (2) (0) (1)`
        - Operation types for each index are below, you do not need to print out them. They are just here to help you understand this example:[C: comparison, R: read, W: write]
            ` C   C   C   C   R   W   R   W   C   C   C   C   C   C   C   C `
   * [Bonus] Harden the seeing of your bubble sort so that under different input it produces indices access trace.
     
#### Homework Lab (3): Other Sorting algorithms
        
3. Task 3 is about implementing randomized quick sort, and observing access trace.
    * Try to run at least two different arrays as input. Observe the output difference between different inputs. Explain your found.
          - For instance, [18,17,16,15,14,13,12,11] and [18,11,12,17,16,14,13,15]

    * [Bonus] Discuss the Side-channel leakage you found.
          - Analyse whether the trace leaky? If it is, think of an attack scheme with high success rate or even a protection scheme; if not, justify your claim