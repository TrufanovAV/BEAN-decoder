# Protocol decoder BEAN for Open-Source signal analysis software http://sigrok.org/
<p align="center">
  <img alt="VS Code in action" src="https://cloud.githubusercontent.com/assets/11839736/16642200/6624dde0-43bd-11e6-8595-c81885ba0dc2.png">
</p>

BEAN is a Toyota's Body Electronics Area Network:
* 1 SOF (1 bit) (start of frame)
* 2-5 PRI (4 bits) (priority of bits)
* 6-9 ML (4 bits) (message length, length of message and number of bytes, [ID + DATA] (3-13)
* 10-17 DST-ID (8 bits) (the id showing the communication destination)
* 18-25 MES-ID (8 bits) (is an area showing message contents)
* 26-113 DATA (8 to 88 bits) (1 to 11 bytes of data, variable length, could be as small as bits 26-33, reskewing the bit number slots for the remaining bit groups)
* 114-121 CRC (8 bits) (error check code remainder from polynomial)
* 122-129 EOM (8 bits) (end of message and controls time to prepare response for sending)
* 130-132 RSP (2 bits) (represents the response area)
* 133-138 EOF (6 bits) (shows the end of the frame)

    PRI
    |                           DATA (variable                   RSP
 SOF|    ML   DID      MID      | length)      CRC      EOM      |  EOF
  | |    |    |        |        |              |        |        |  |
  x_xxxx_xxxx_xxxxxxxx_xxxxxxxx_[8 to 88 bits]_xxxxxxxx_xxxxxxxx_xx_xxxxxx 


* For example:

*    PRI+ML	 DST-ID   MES-ID   DATA 1   DATA 2   CRC     EOM 
*    04	 	 FE		  AB	   A1	    80	     E9	     7E

*  1000001100 111110110 1010101110100001100000100111010010111111   with bit stuff
*   00000100 11111110 10101011 10100001 10000000 11101001 01111110  remove bit stuff
