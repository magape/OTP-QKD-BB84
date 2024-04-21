# Encryption in Python
## QKD: quantum key, ultimate secrecy.

Mihai Agape

Palatul Copiilor Drobeta Turnu Severin

The purpose of the **Encryption in Python** project is to write code to encrypt and decrypt a message—**QKD: quantum key, ultimate secrecy.**—using a key created through Quantum Key Distribution (**QKD**). In this paper I analyze the work I have done, describing the steps of encryption and decryption the data. I also discuss what effect an eavesdropper would have on the success of transmitting encrypted data.

## Introduction
In this section I provide a description of the functions for encryption and decryption the message.

The encryption function—`encrypt(message, key)`—encrypts the `message` with the `key`, using **OTP** (One Time Pad) algorithm. The encryption function has two parameters: the message, and the key. The message is an **ASCII string** representing the information which the sender (e.g. Alice) wants to send to the receiver (e.g. Bob). Each ASCII character is coded with seven bits. A valid message must contain just ASCII characters. The key is a **bitstring* generated in advance using QKD **BB84** protocol. The length of the key must be seven times the length of the message. The encryption function returns the ciphertext as an ASCII string. The encryption function doesn't check if the message and the key are valid.

The decryption function—`decrypt(ciphertext, key)`—decrypts the `ciphertext` (returned by the encryption function) using the same `key` as the key used for encryption. The decryption function has two parameters, the ciphertext, and the key. The ciphertext is an **ASCII string** representing the encrypted information which the sender obtained from the plain message and the key with the encryption function. Each ASCII character of the ciphertext is coded with seven bits. The length of the key is seven times the length of the ciphertext. The decryption function returns the plain message (used by sender as argument of the encryption function) as an ASCII string.

As can be seen above, after the generation of key using QKD, the communication doesn't involve quantum processing. The encryption and decryption operations use the QKD generated key but are implemented with classical computing devices. The transmission of the ciphertext between sender and receiver also is done through a classical channel.

## Code Snippets
In this section I describe how I implemented the functions for encryption and decryption of the message.
### Message Encryption
Next is the definition of the encryption function, excluding the comments.
```python
def encrypt(message, key):
  bitstring_message = ''.join([f'{ord(char):07b}' for char in message])
  bitstring_cipher = ''.join(str(int(m) ^ int(k)) for m, k in zip(bitstring_message, key))
  ciphertext = ''.join(chr(int(bitstring_cipher[i:i+7], 2)) for i in range(0, len(bitstring_cipher), 7))
  return ciphertext  
```
The encryption function is implemented in four steps:
1.	The ASCII string **message** is converted to the bitstring message (**bitstring_message**). One ASCII character is encoded with 7 bits.
2.	The bitstring cipher (**bitstring_cipher**) is created by bitwise XOR-ing the bitstring message (**bitstring_message**) with the bitstring **key**.
3.	The bitstring cipher (**bitstring_cipher**) is converted to the ASCII string **ciphertext**.
4.	The encryption function returns the **ciphertext**.

### Message Decryption
Next is the definition of the decryption function, excluding the comments.
```python
def decrypt(ciphertext, key):
  return encrypt(ciphertext, key)
```
The decryption function is the inverse of encryption function. Because the encryption function creates the ciphertext as bitwise XOR between message and key, the inverse of the encryption function is the bitwise XOR between ciphertext and key. Therefore, the decryption is implemented with encryption function: `decrypt(ciphertext, key) = encrypt(ciphertext, key)`.

### Testing the Functions
I tested the encryption and decryption functions with the message **QKD: quantum key, ultimate secrecy.**, using the code below.
```python
message = 'QKD: quantum key, ultimate secrecy.'
key = ''.join(choice('01') for _ in range(7 * len(message)))
ciphertext = encrypt(message, key)
plain_text = decrypt(ciphertext, key)

print(f'message: \t\t{message}')
print(f'key (bitstring): \t{key}')
print(f'key (ASCII): \t\t{"".join(chr(int(key[i:i+7], 2)) for i in range(0, len(key), 7))}')
print(f'ciphertext: \t\t{ciphertext}')
print(f'plaintext: \t\t{plain_text}')
```
A random key of appropriate length was generated using the `choice` function of the `random` module. The key is displayed as bitstring and also as text string. The displayed ciphertext is not intelligible. The decryption function returns the original message.

```
message: 		QKD: quantum key, ultimate secrecy.
key (bitstring): 	01101110101101111111100010001101101010001001110100100111010101111111111100111011001011011001111011111001100110000011000110001011111010100011001101001101011111100101111011001000011110111100001011011111101101111101101111010010001110001011100000010
key (ASCII): 		7-m":'+g2l{sb}#5|^dx-}_7R.
ciphertext: 		ff;2MSOFE_La4On\?{X^<E7W,
plaintext: 		QKD: quantum key, ultimate secrecy.
```
### Other Functions
Other useful functions are:
- `input_message()`, which takes the message from the user (i.e. sender); if the sender's message includes non ASCII characters, the function requests a new message from the sender; the function returns the sender's message if the message has just ASCII characters.
- `get_key()`, which simulates all the BB84 QKD protocol; the protocol returns a bitstring key if there is not an active eavesdropper.

## Analysis
In this section I analyze what is the impact of an eavesdropper on the encryption and decryption of data.

If an eavesdropper tries to intercept the quantum communication during the QKD, the sender and receiver will discover the eavesdropper and the key will not be generated. The sender and receiver will generate a QKD key just if there is not an active eavesdropper on the quantum channel. That means that the quantum process guarantees that the correct key is shared just between the sender and receiver.

If an eavesdropper tries to intercept the encrypted message (sent through a classical channel) there is not a problem. The OTP encryption guarantees that it is impossible for someone to decrypt the message without the correct key.

Of course, the QKD and OTP doesn’t help if an eavesdropper has access to the devices / computers of the sender and / or receiver.

## Conclusion

Traditional cryptography methods rely on complex mathematical problems that are difficult to solve, but we are not sure how much time will remain so.

The quantum cryptography utilizes the laws of quantum mechanics to distribute keys (QKD) which with OTP encryption create unbreakable codes. This combination (QKD & OTP) provides:

- unbreakable security (impossible to eavesdrop on communication without detection);
- future-proof security (secure even against powerful quantum computers of the future).

While still in its early stages, quantum encryption promises to secure communication in the quantum age.
