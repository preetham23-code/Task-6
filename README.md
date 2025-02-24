# Task-6
Implementation of 2-bit Comparator using VSDSquadron Mini Board

Overview
This project aims to design and implement a 2-bit comparator using the VSDSquadron Mini board. A 2-bit comparator is a digital circuit that compares two 2-bit binary numbers and indicates whether one number is greater than, less than, or equal to the other. The project involves designing the comparator logic using C programming in Visual Studio Code, setting up the hardware connections on a breadboard, and verifying the functionality through LEDs connected
Give me project explanation working and conclusion for this project 
ChatGPT said:
Certainly! Here's a detailed project explanation, working, and conclusion for designing and implementing a 2-bit comparator using the VSDSquadron Mini board:

Project Explanation
Objective: Design and implement a 2-bit comparator to compare two 2-bit binary numbers using the VSDSquadron Mini board.

Components:

VSDSquadron Mini board: This serves as the main microcontroller platform for executing the comparator logic.
Breadboard: Used for connecting components such as LEDs to visually indicate the comparison results.
LEDs: Indicators to show whether the first number is greater than, less than, or equal to the second number.
Resistors and wires: Required for proper connections and current limiting for LEDs.
Comparator Logic Design:

The 2-bit comparator compares two 2-bit binary numbers A and B.
It checks if A is greater than, less than, or equal to B.
This is achieved by comparing each bit of A with the corresponding bit of B.
Implementation Steps:

C Programming in Visual Studio Code:

Write the logic for the comparator in C programming language.
Use bitwise operators to compare individual bits of A and B.
Hardware Setup on Breadboard:

Connect the VSDSquadron Mini board to the breadboard.
Connect LEDs to the digital output pins of the board to represent the comparison results (greater than, less than, equal).
Testing and Verification:

Upload the compiled program to the VSDSquadron Mini board.
Input different combinations of 2-bit binary numbers (A and B).
Observe the LEDs to verify if they correctly indicate the comparison results.
Working
Programming the Comparator Logic:

Utilize C programming to define functions that perform bitwise operations to compare the two 2-bit numbers.
Setting Up the Hardware:

Connect the appropriate pins of the VSDSquadron Mini board to the breadboard.
Attach LEDs with current-limiting resistors to the output pins of the board.
Execution:

Execute the program on the board.
Input binary numbers A and B via switches or through defined input methods.
LED Indication:

LEDs light up according to the result of the comparison:
One LED for "A > B".
One LED for "A < B".
One LED for "A = B".
Conclusion
Achievements: Successfully designed and implemented a 2-bit comparator using the VSDSquadron Mini board.
Functionality Verification: The comparator accurately determines whether one 2-bit number is greater than, less than, or equal to another.
Challenges Overcome: Overcame challenges in programming the logic and ensuring correct hardware connections.
Future Improvements: Potential enhancements include expanding the comparator to handle larger bit numbers and optimizing the code for efficiency.
This project not only demonstrates understanding of digital logic and microcontroller programming but also provides hands-on experience with hardware interfacing and debugging.

#include <stdio.h>
#include <debug.h>
#include <ch32v00x.h>

// Configures GPIO Pins for Comparator Operation
void GPIO_Config(void) {
    GPIO_InitTypeDef GPIO_InitStructure = {0}; // GPIO configuration structure
    
    // Enable clock for GPIO ports
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE);
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE);
    
    // Configure input pins (A and B)
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_1 | GPIO_Pin_2;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU; // Input Pull-Up
    GPIO_Init(GPIOD, &GPIO_InitStructure);

    // Configure output pins (A > B, A < B, A == B)
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_4 | GPIO_Pin_5 | GPIO_Pin_6;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP; // Output Push-Pull
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz; // GPIO speed
    GPIO_Init(GPIOC, &GPIO_InitStructure);
}

// Main function
int main() {
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_1);
    SystemCoreClockUpdate();
    Delay_Init();
    GPIO_Config();

    while(1) {
        // Output the results
        // A > B
        if(GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_1) == RESET && GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_2) == SET) {
            GPIO_WriteBit(GPIOC, GPIO_Pin_4, SET); // Set A > B pin high
        } else {
            GPIO_WriteBit(GPIOC, GPIO_Pin_4, RESET); // Set A > B pin low
        }

        // A < B
        if(GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_1) == SET && GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_2) == RESET) {
            GPIO_WriteBit(GPIOC, GPIO_Pin_5, SET); // Set A < B pin high
        } else {
            GPIO_WriteBit(GPIOC, GPIO_Pin_5, RESET); // Set A < B pin low
        }

        // A == B
        if(GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_1) == GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_2)) {
            GPIO_WriteBit(GPIOC, GPIO_Pin_6, SET); // Set A == B pin high
        } else {
            GPIO_WriteBit(GPIOC, GPIO_Pin_6, RESET); // Set A == B pin low
        }
    }

    return 0;
}
