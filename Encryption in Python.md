# Encryption in Python
## QKD: quantum key, ultimate secrecy

Mihai Agape

Palatul Copiilor Drobeta Turnu Severin

The purpose of the **Encryption in Python** project is to write code to encrypt and decrypt a message—**QKD: quantum key, ultimate secrecy.**—using a key created through Quantum Key Distribution (**QKD**). In this paper I analyze the work I have done, describing the steps of encryption and decryption the data. I also discuss what effect an eavesdropper would have on the success of transmitting encrypted data.

## Introduction
In this section I provide a description of the functions for encryption and decryption the message.

The encryption function—**encrypt(message, key)**—encrypts the **message** with the **key**, using OTP (One Time Pad) algorithm. The encryption function has two parameters: the message, and the key. The message is an ASCII string representing the information which the sender (e.g. Alice) wants to send to the receiver (e.g. Bob). Each ASCII character is coded with seven bits. A valid message must contain just ASCII characters. The key is a bitstring generated in advance using QKD BB84 protocol. The length of the key must be seven times the length of the message. The encryption function returns the ciphertext as an ASCII string. The encryption function doesn't check if the message and the key are valid.

The decryption function—**decrypt(ciphertext, key)**—decrypts the **ciphertext** (returned by the encryption function) using the same **key** as the key used for encryption. The decryption function has two parameters, the ciphertext, and the key. The ciphertext is an ASCII string representing the encrypted information which the sender obtained from the plain message and the key with the encryption function. Each ASCII character of the ciphertext is coded with seven bits. The length of the key is seven times the length of the ciphertext. The decryption function returns the plain message (used by sender as argument of the encryption function) as an ASCII string.

As can be seen above, after the generation of key using QKD, the communication doesn't involve quantum processing. The encryption and decryption operations use the QKD generated key but are implemented with classical computing devices. The transmission of the ciphertext between sender and receiver also is done through a classical channel.

## Code Snippets
In this section I will provide how I implemented the functions for encryption and decryption of the message.
### Message Encryption
Next is the definition of the encryption function.
'''python
def encrypt(message, key):
  bitstring_message = ''.join([f'{ord(char):07b}' for char in message])
  bitstring_cipher = ''.join(str(int(m) ^ int(k)) for m, k in zip(bitstring_message, key))
  ciphertext = ''.join(chr(int(bitstring_cipher[i:i+7], 2)) for i in
                range(0, len(bitstring_cipher), 7))
  return ciphertext  
'''
Convert ASCII message to bitstring (one char is encoded with 7 bits)
Perform XOR bitwise between message and key
Convert bitstring to ASCII text, i.e. ciphertext
encrypted_message coded as ASCII text

The functions for encryption and decryption my message.
Provide a description of the functions for encryption and decryption your message including: any arguments they will take, what they will return, and the general approach they will use to accomplish this.

The encryption function is implemented in four steps:
1.	The ASCII string message is converted to the bitstring message. One ASCII character is encoded with 7 bits.
2.	The bitstring cipher is created by bitwise XOR-ing the bitstring message with the bitstring key.
3.	The bitstring cipher is converted to the ASCII string ciphertext.
4.	The encryption function returns the ciphertext.
The ciphertext is sent by the sender to the receiver using a classical channel.

### Message Encryption
### Message Decryption

The decryption function is the inverse of encryption function. Because the encryption function creates the ciphertext as bitwise XOR between message and key, the inverse of the encryption function is the bitwise XOR between ciphertext and key. Therefore, the decryption is implemented with encryption function: decrypt(ciphertext, key) = encrypt(ciphertext, key).

### Testing My Functions
### Other Functions
#### Input the message
#### BB84 Protocol

## Analysis
How would an eavesdropper impact the encryption and decryption of my data?

Finally, I have written a two pages paper detailing the steps I took to write your encryption and decryption functions, explaining what would happen if there were an eavesdropper when the key was first being developed and how that would affect the encryption and decryption of the message, and why developing QKD and other quantum encryption algorithms may be important to the future (this may require additional research on your part).
## Conclusion
Why is the development of quantum encryption important for the future of secure communication?


