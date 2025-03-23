# a07g-exploring-the-CLI

* Team Number:
* Team Name:
* Team Members:
* GitHub Repository URL:
* Description of test hardware: (development boards, sensors, actuators, laptop + OS, etc)

## 1. Software Architecture

![1742673035756](image/A07G_README/1742673035756.png "System Tasks OverView")

![1742673317865](image/A07G_README/1742673317865.png)

![1742673391697](image/A07G_README/1742673391697.png)

![1742673410665](image/A07G_README/1742673410665.png)

![1742673450993](image/A07G_README/1742673450993.png)

![1742673469286](image/A07G_README/1742673469286.png)

![1742673490798](image/A07G_README/1742673490798.png)

 ![1742673521412](image/A07G_README/1742673521412.png)

## 2. Understanding the Starter Code

1. **What does “InitializeSerialConsole()” do? In said function, what is “cbufRx” and “cbufTx”? What type of data structure is it?**
   The `InitializeSerialConsole()` function initializes the serial console by initializing two circular buffers, `cbufRx` and `cbufTx`, using the `circular_buf_init()` function with `rxCharacterBuffer` and `txCharacterBuffer` respectively. It also configures the USART using `configure_usart()`, registers the read and write callbacks using `configure_usart_callbacks()`, sets the interrupt priority for `SERCOM4_IRQn`, and starts a continuous reading process using `usart_read_buffer_job()`. `cbufRx` is a circular buffer handle for receiving characters, and `cbufTx` is a circular buffer handle for transmitting characters.
2. **How are “cbufRx” and “cbufTx” initialized? Where is the library that defines them (please list the \*C file they come from).**
   `cbufRx` is initialized by calling `circular_buf_init((uint8_t *)rxCharacterBuffer, RX_BUFFER_SIZE)`, and `cbufTx` is initialized by calling `circular_buf_init((uint8_t *)txCharacterBuffer, TX_BUFFER_SIZE)`. Based on common practices and the function name, the library that defines `circular_buf_init` and the `cbuf_handle_t` type is likely located in a file named `circular_buffer.c`, although this is not explicitly stated in the provided `SerialConsole.c` file.
3. **Where are the character arrays where the RX and TX characters are being stored at the end? Please mention their name and size.**
   The character arrays where the RX and TX characters are stored are `rxCharacterBuffer` with a size of `RX_BUFFER_SIZE` (defined as 512 bytes) and `txCharacterBuffer` with a size of `TX_BUFFER_SIZE` (defined as 512 bytes).
4. **Where are the interrupts for UART character received and UART character sent defined?**
   The code sets the priority for the `SERCOM4_IRQn` interrupt, which is the interrupt associated with the SERCOM4 peripheral used for UART communication. The specific definition of the interrupt handlers would be in the microcontroller's startup files or within the SERCOM driver implementation. The callbacks `usart_read_callback` and `usart_write_callback` are registered for the `USART_CALLBACK_BUFFER_RECEIVED` and `USART_CALLBACK_BUFFER_TRANSMITTED` events, respectively, in the `configure_usart_callbacks()` function.
5. **What are the callback functions that are called when: A character is received? (RX) A character has been sent? (TX)**
   When a character is received (RX), the `usart_read_callback` function is registered to be called. When a character has been sent (TX), the `usart_write_callback` function is registered to be called.
6. **Explain what is being done on each of these two callbacks and how they relate to the cbufRx and cbufTx buffers.**
   The `usart_read_callback` function is intended to handle the reception of characters. The comment `// ToDo: Complete this function` indicates that the logic to store the received character (likely `latestRx`) into the `cbufRx` buffer needs to be implemented. The `usart_write_callback` function is called when the transmission of a buffer is complete. It checks if there are more characters in the `cbufTx` buffer using `circular_buf_get()`. If there are more characters, it gets the next character and initiates its transmission using `usart_write_buffer_job()`.
7. **Draw a diagram that explains the program flow for UART receive – starting with the user typing a character and ending with how that character ends up in the circular buffer “cbufRx”. Please make reference to specific functions in the starter code.**

   ![1742681146871](image/A07G_README/1742681146871.png)
8. **Draw a diagram that explains the program flow for the UART transmission – starting from a string added by the program to the circular buffer “cbufTx” and ending on characters being shown on the screen of a PC (On Teraterm, for example). Please make reference to specific functions in the starter code.**

   ![1742681166227](image/A07G_README/1742681166227.png)
9. **What is done on the function “startStasks()” in main.c? How many threads are started?**
   The function `StartTasks()` (note the capitalization in the code) is defined in `main.c`. This function first prints the heap size before starting tasks. Then, it creates one FreeRTOS task using `xTaskCreate()`: `vCommandConsoleTask` with the name "CLI\_TASK". After creating this task, it prints the heap size again. Therefore, based on the provided `main.c` code, **one thread** is explicitly started within the `StartTasks()` function.

## 4. Wiretap the convo!

1. Based on the SerialConsole.c code and the SAMW25_XPLAINED_PRO.h file, the UART communication for the EDBG CDC interface uses SERCOM4. The specific pins are defined by:
   EDBG_CDC_SERCOM_PINMUX_PAD2 which corresponds to the Transmit (TX) line,PB10_SERCOM_4. EDBG_CDC_SERCOM_PINMUX_PAD3 which corresponds to the Receive (RX) line.PB11_SERCOM4.
2. From the following figure, we can see we need attach the pin PB10 and PB11.

![1742764585479](image/A07G_README/1742764585479.png)

![1742765077883](image/A07G_README/1742765077883.png)

### 2.**photo of your hardware connections**

![1742764411300](image/A07G_README/1742764411300.png)

### 3.**screenshot of the decoded message**

![1742765185317](image/A07G_README/1742765185317.png)

### 4. **Capture File**

The usart.sal is our capture file.

## 6. **Add CLI commands**

video link: [https://youtu.be/FAAj1BERWvg?si=ajbn2K48HhYb-9YP]()
