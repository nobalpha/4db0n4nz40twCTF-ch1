# adventbonanzaotwCTF-ch1
Welcome dear programmer, I salute you and want to learn your thoughts and feedbacks at this challenge :) Just take a look...

import csv
import enum
class Keys(enum.Enum):
  N7110_KEYPAD_ZERO = 0
  N7110_KEYPAD_ONE = 1
  N7110_KEYPAD_TWO = 2
  N7110_KEYPAD_THREE = 3
  N7110_KEYPAD_FOUR = 4
  N7110_KEYPAD_FIVE = 5
  N7110_KEYPAD_SIX = 6
  N7110_KEYPAD_SEVEN = 7
  N7110_KEYPAD_EIGHT = 8
  N7110_KEYPAD_NINE = 9
  N7110_KEYPAD_STAR = 10
  N7110_KEYPAD_HASH = 11
  N7110_KEYPAD_MENU_LEFT = 100
  N7110_KEYPAD_MENU_RIGHT = 101
  N7110_KEYPAD_MENU_UP = 102
  N7110_KEYPAD_MENU_DOWN = 103
  N7110_KEYPAD_CALL_ACCEPT = 104
  N7110_KEYPAD_CALL_REJECT = 105

keysWithText = [
  " 0",
  ".,'?!\"1-()@/:",
  "abc2",
  "def3",
  "ghi4",
  "jkl5",
  "mno6",
  "pqrs7",
  "tuv8",
  "wxyz9",
  "@/:_;+&%*[]{}",
  "N7110_IME_METHODS"
]
csv_buffer = []
hashcounter = 0
charcounter = 0
temp = 0
with open('sms4.csv') as csvf:
    csv_buffer+=csv.reader(csvf,quotechar='|')

with open('exp.txt','w') as exp:
  with open('out.txt','w') as f:
    for row in range(len(csv_buffer)):
      for key in Keys:
        if int(csv_buffer[row][1]) == key.value and hashcounter != 2:
          if key.value == 11: #Bastığı şey # ise sayısını bir arttır
            hashcounter += 1
        else:
          if int(csv_buffer[row][1]) == key.value:
            if len(keysWithText) < key.value and int(csv_buffer[row][1]) != 100 :
              pass
            else:
              try:
                if charcounter > len(keysWithText[key.value]) - 1:
                  charcounter = (charcounter % len(keysWithText[key.value]))
                if row == len(csv_buffer) - 1: #Eğer son eleman ise direkt yazdır
                  f.write(str(keysWithText[key.value])[charcounter])
                elif csv_buffer[row][1] != csv_buffer[row+1][1]: 
                  f.write(str(keysWithText[key.value])[charcounter])
                  charcounter=0
                elif csv_buffer[row][1] == csv_buffer[row+1][1] and int(csv_buffer[row+1][0])   - int(csv_buffer[row][0]) < 880 : 
                  charcounter += 1
                else:
                  f.write(str(keysWithText[key.value])[charcounter])
                  charcounter=0
                exp.write("Value uniqueness: {0} Timeout duration: {1} Row: {2} Charcounter: {3} Key.Value: {4}\n".format(csv_buffer[row][1] != csv_buffer[row+1][1],(int(csv_buffer[row+1][0]) - int(csv_buffer[row][0]) > 900),row,charcounter,key.value))
              except Exception as e:
                print(e)
              
  print("Program finished.")
      
