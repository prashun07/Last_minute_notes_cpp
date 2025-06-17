# BIT MANIPULATION IN C FOR EMBEDDED SYSTEMS
```c
>> // Right shift
<< // Left shift
&  // Bitwise AND
|  // Bitwise OR
^  // Bitwise XOR
~  // Bitwise complement
```


```c
// REG_A - Is a example Micro-controller peripheral register,
//         that is memory mapped into a memory address.
//         They can be found in the datasheet of the MCU,
//         in the section of the registers of the specific
//         peripheral and there, you can find also the info
//         on the functionality of every bit in it.
//         Some register are Write only, some are Read only,
//         some of them are Read and Write.
//         The Datasheet, User Guide and Erratas of them
//         for the MCU are the truth that will guide you! 


// SET BIT - Set Bit 2 to 1 (True) on REG_A with OR:
   REG_A |= (1 << 2);

   // Set bit 2 and bit 4 to 1 (True):
   REG_A |= ( (1 << 2) | (1 << 4) );   


// CLEAR BIT - Clear Bit 2 to 0 (False) on REG_A with AND and NOT:
   REG_A &= ~(1 << 2);

   // Clear bit 2 and bit 4 to 0 (False):
   REG_A &= ~( (1 << 2) | (1 << 4) );


// TOGGLE BIT - Toggle or flip bit 2 on REG_A with XOR:
   REG_A ^= (1 << 2); 

   // Flip bit 2 and bit 4 from 0 -> 1 or from 1 -> 0:
   REG_A ^= ( (1 << 2) | (1 << 4) );



// Make a one bit light up from right to left and from left to right:

#define DELAY_TIME 300  /* ms */

// PORTB is a 8 bit ports.
REG_A = 0;   // Clears REG_A.
uint8_t i;

// From right to left.
for(i=0; i < 8; i++) {
   REG_A |= (1 << i);       // Set bit.
   _delay_ms(DELAY_TIME);
   REG_A &= ~(1 << i);      // Clear bit.
}

// From left to right.
for(i=7; i < 255; i--) {
   REG_A |= (1 << i);       // Set bit.  
   _delay_ms(DELAY_TIME);
   REG_A &= ~(1 << i);      // Clear bit.
}

// Note: In the place of i < 255,
//       can't use i < 0 neither i <= 0.
//       The last one will give you
//       a infinite loop because of underflow. 

```
---
**How to test if the bit of register is set to 1 or 0?**
```c
// Test if the 2 bit is 1 (True).

// REG_A   : xxxx xxxx
// (1 << 2): 0000 0100
//       & : 0000 0x00

// In C, "if" is FALSE if is zero
// and TRUE for all other values. 

if ( REG_A & ( 1<<2 ) ) {
  do_something();
}


#define BIT_SHIFT(bit)         ( 1 << (bit) ) 
#define BIT_IS_SET(reg, bit)   ( (reg) & BIT_SHIFT(bit) ) 
#define BIT_IS_CLEAR(reg, bit) ( !( (reg) & BIT_SHIFT(bit) ) ) 
#define LOOP_UNTIL_BIT_IS_SET(reg, bit)   do { } while ( BIT_IS_CLEAR(reg, bit) ) 
#define LOOP_UNTIL_BIT_IS_CLEAR(reg, bit) do { } while ( BIT_IS_SET(reg, bit) ) 

if (BIT_IS_SET(REG_A, 2)) {
   do_something();  
}
```

---
```c
// To set a number or bit mask, 
// by clear the first 4 bit's
// and set the first 2 bit's to
// the number 3 (0000 0011) do:

#define PC3 0x3 // 0000 0011

REG_A = REG_A & 0b11110000;   // clear all 4 bits
REG_A = REG_A | PC3;          // set the bits we need 

// More compact ways of doing it. 

REG_A = (0b11110000 & REG_A) | PC3; 

// or
 
REG_A = (0xf0 & REG_A) | PC3;

// or

REG_A = (0xf0 & REG_A) | 3;


// Important Note: 
//   The bit manipulation notes are modified
//   versions from the fantastic book:
//
//   Make: AVR Programming by Elliot Williams
//   
//   A can't recommend enough this book to all future embedded
//   or micro-controller developers because it's a really
//   important stepping stone in your journey
//   for every kind, brand and family of MCU not
//   just AVR. This book is that good!
//
//   With it, you will learn 6 things, principles:
//      1. - How to program a MCU just from registers.
//      2. - How to read and use a MCU datasheet.
//      3. - How to the set of most common MCU peripherals work
//           and how to use them well.
//      4. - Bit fiddling and how to use timers and interrupts well.
//      5. - Good practices of MCU development
//           and advanced stuff like ADC's, SPI, I2C, clocks and more.
//      6. - Some really cool projects.
//
// Final note: This book is a labour of love
//             and you can see it in every word of it!
//             Make yourself a favour and grab a copy of it!

```

**Simple debounce routine**

```c
#define DEBOUNCE_TIME  1000    /* ms */
#define BUTTON_PIN_A      1

uint8_t debounce_button_A() {
  if ( BIT_IS_CLEAR( BUTTON_PIN_PORT_REG, BUTTON_PIN_A ) ) {
     // The button has a pull-up resistor
     // so it's active low, and we test if it is zero,
     // here it is pressed.
     _delay_us( DEBOUNCE_TIME );
     if ( BIT_IS_CLEAR( BUTTON_PIN_PORT_REG, BUTTON_PIN_A ) ) {
       // We wait 1 ms and see if it is still pressed,
       // after the elapsed time and we return the state
       // of the button.
       return(1);
     }
  }
  return(0);
} 


// To use do:

if (debounce_button_A()) {
   do_something();   
}

```

## C for Embedded Systems

```c
// Include that has the primitive fixed size types.
#include <stdint.h>

// How to define registers based on the Memory Map base address of the peripheral and a specific register offset of that peripheral?
// Register that Enables the RCC peripheral Clock.
#define RCC_BASE_ADDR          0x40012000UL
#define RCC_APB2_ENR_OFFSET    0x44UL
#define RCC_APB2_ENR_ADDR      (RCC_BASE_ADDR + RCC_APB2_ENR_OFFSET)

// Register of the ADC enable of random bit.
#define ADC_BASE_ADDR          0x42012000UL
#define ADC_CR1_REG_OFFSET     0x04UL
#define ADC_CR1_REG_ADDR       (ADC_BASE_ADDR + ADC_CR1_REG_OFFSET)

int main(void){

    // How to SET and Clear bit's on a register?

    // The following case makes a set of one bit to '1'.

    // Register that enables the RCC peripheral Clock on bit 9th.
	uint32_t volatile *pRccApb2Enr = (uint32_t *) RCC_APB2_ENR_ADDR;
	*pRccApb2Enr |= (1 << 8)


    // The following case makes a clear of one bit to '0'.

    // Register of the ADC that we will clear the 9th bit.
	uint32_t volatile *pAdcCr1Reg = (uint32_t *) ADC_CR1_REG_ADDR;
	*pAdcCr1Reg &= ~(1 << 8)


    // The following case makes the Clear of the value 3 -> ~'11' that is '00' 
    // and set's the value 2 -> '10'.

    uint32_t volatile * pGPIOAModeReg = (uint32_t *) (GPIOA_BASE_ADDR + 0x00)  // + offset address.

    *pGPIOAModeReg &= ~(0x3 << 16);    // **Clear**
    *pGPIOAModeReg |=  (0x2 << 16);    // **Set**

    // How to test if one bit transition from 0 to 1, that is bit 18?

    // We test if one bit transition from 0 to 1, that is bit 18.    
    while( !(*pGPIOA_IOR_Reg & (1 << 18)) );
}

```

## Keyword **const** and **volatile**

1. The keyword **const** means that the it is constant, the content data or the address of the pointer. 

2. The keyword **volatile** means that the content data or the address of the pointer is never optimized away by the compiler -O2 or something, because each time it is used a fresh copy of the data as to be fetched from memory. It is very useful, when dealing with registers on a microController or when several simultaneous threads are executing, or when a main thread is executing and a ISR will interrupt the main processing thread. Because code can be interrupted every access to this volatile variable should be fetched again from the memory or memory mapped register. 

```c
#define SRAM_ADDRESS1    0x20000000UL

// This means that the content of the pointer is "volatile".
uint32_t volatile *p = (uint32_t *) SRAM_ADDRESS1;

// This means that the address of the pointer is "volatile".
uint32_t * volatile p = (uint32_t *) SRAM_ADDRESS1;

// This means that the content of the data is volatile and the address of the pointer is "volatile".
uint32_t volatile * volatile p = (uint32_t *) SRAM_ADDRESS1;


// ... and the same logic and places to the keyword "const".  

```

## Sizes for 32 bit microController

- **Byte**        : data of 8-bit length.
- **Half word**   : data of 16-bit length.
- **Word**        : data of **32-bit** length. Because the microController is a 32 bit's microController.
- **Double word** : data of 64-bit length.


## Two ways of making a Menu with strings - char * - and send it to UART.

1. The complex way...

```c
	// Menu text to TX:
	char const * message_0 = "\n\n\n   Menu\n\n";
	char const * message_1 = "1. LED animation.\n";
	char const * message_2 = "2. Option 2.\n";
	char const * message_3 = "3. Option 3.\n";
	char const * message_4 = "4. Option 4.\n\n";
	char const * all_messages[] = {message_0, message_1, message_2, message_3, message_4 };

	uint32_t all_messages_size = sizeof(all_messages) / sizeof(all_messages[0]);
	for(int i=0; i < all_messages_size; i++){
		HAL_UART_Transmit(&huart2, (uint8_t *) all_messages[i], (uint16_t) strlen(all_messages[i]),
                          timeout);
	}

```

2. The simple way ...

```c
    #include <string.h>

	// Menu text to TX:
	char const * message = "\n\n\n   Menu\n\n"
	                       "1. LED animation.\n"
	                       "2. Option 2.\n"
	                       "3. Option 3.\n"
	                       "4. Option 4.\n\n";
	
	HAL_UART_Transmit(&huart2, (uint8_t *) message, (uint16_t) strlen(message),
                      timeout);
	
```

# Utils

## Abstract Double Linked List - jco_list

This is a Abstract Double Linked List, called jco_list and it's license is MIT Open Source. <br>
The list interface follows. To see examples of the list usage see the tests inside the main.c source code file.

```c
typedef struct node{
    void * elem;
    struct node * next;
    struct node * prev;
} NODE;

typedef struct{
    NODE * firstNode;
    NODE * lastNode;
    int size;
    NODE * iterator;
} LST;

typedef enum { LST_D_UP, LST_D_DOWN } LST_DIRECTION;


LST *  lst_new(/* NULL or pointer to function equals == */);
bool   lst_free(LST * lstObj);

int    lst_size(LST * lstObj);

void * lst_get_first(LST * lstObj);
void * lst_get_last(LST * lstObj);
void * lst_get_at(LST * lstObj, int pos);

// Returns the previous existing element in that pos.
void * lst_set(LST * lstObj, void * elem, int pos);

bool   lst_insert_first(LST * lstObj, void * elem);
bool   lst_insert_last(LST * lstObj, void * elem);
bool   lst_insert_at(LST * lstObj, void * elem, int pos);

// Receives a pointer to a function compare that as 
// 2 parameters "a" and "b" and returns int,
// 1 if a > b, 0 if a == b and -1 if a < b.
 bool   lst_insert_ordered(LST * lstObj, void * elem,
                          int (* const ptr_to_funct_cmp) (void * a, void * b ) );

void * lst_remove_first(LST * lstObj);
void * lst_remove_last(LST * lstObj);
void * lst_remove_at(LST * lstObj, int pos);

// Iterators NEXT and PREV.
bool   lst_iter_get_first(LST * lstObj);
bool   lst_iter_get_last(LST * lstObj);
void * lst_iter_next(LST * lstObj);
void * lst_iter_prev(LST * lstObj);
bool   lst_iter_is_begin(LST * lstObj);
bool   lst_iter_is_end(LST * lstObj);

void * lst_find(LST * lstObj, void * elem, 
                int (* const ptr_to_funct_cmp) (void * a, void * b),
                LST_DIRECTION direction,
                NODE * currNode,
                NODE ** foundNode);

int cmp_int(void * aIn, void * bIn);
int cmp_float(void * aIn, void * bIn);
int cmp_double(void * aIn, void * bIn);
int cmp_single_char(void * aIn, void * bIn);
int cmp_null_term_str(void * aIn, void * bIn);

```