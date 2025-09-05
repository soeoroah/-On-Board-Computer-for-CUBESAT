On Board Computer Report

What I Did
I learned how to navigate and use Docker to access the source files for our microcontroller project. I located all the relevant C++ files, including those for the ADCS controller and microcontroller wrapper. I also executed telemetry_bridge.py to check the telemetry functionality.
________________________________________
Problems I Faced
1.	Missing Header Files
o	The compiler could not find val_rotation.h, val_f2c.h, and val_always.h.
  These headers are part of the Virtual Almanac Library (VAL), which wasn’t available in the container or online.
2.	Broken Class Syntax 
o	Functions like getFaultFlags() were written outside the class or incorrectly, causing the compiler to treat them as non-member functions.
o	This led to errors like:
o	error: non-member function '...' cannot have const qualifier
o	error: 'fault_flags_' was not declared in this scope________________________________________
How I Fixed It
1.	Created Stubs for Missing Headers
o	I made temporary stub files for val_rotation.h, val_f2c.h, and val_always.h.
o	This allowed the project to compile, even though the VAL library’s actual functionality is not applied.
2.	Fixed the Microcontroller Class
o	I researched on how to define a microcontroller.h proper class with member variables and functions.
o	Getter and setter functions were moved inside the class and marked const only where appropriate.
class Microcontroller {
private:
    uint8_t fault_flags_;
    uint32_t control_cycles_;
    int control_mode_;

public:
    uint8_t getFaultFlags() const { return fault_flags_; }
    void setControlMode(int mode) { control_mode_ = mode; }
    // ...other getters and setters
};
3.	Updated the C Wrapper
o	I rewrote microcontroller_wrapper_c.cpp to use a global instance of the Microcontroller class.
o	Functions were wrapped in extern "C" so C code can still call them.
________________________________________
Compilation
Once these fixes were in place, the project compiled successfully using:
g++ -std=c++17 \
-I./files/stubs \
./files/microcontroller_wrapper_c.cpp \
./files/microcontroller.cpp \
./files/adcs_controller.cpp \
-o microcontroller_program
The stubs allowed compilation even without the full VAL library.
________________________________________
Results
I got to learn through all the errors and I explored Docker and its functionalities. The simulation however doesn’t compile

________________________________________
Tools I used
https://www.doulos.com/training/arm-and-embedded-software/embedded-ccplusplus/modern-cplusplus-for-embedded-microcontrollers/
https://blog.mbedded.ninja/programming/languages/c-plus-plus/cpp-on-embedded-systems/
https://www.perplexity.ai/
https://chatgpt.com/
