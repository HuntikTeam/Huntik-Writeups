### Challenge : Merry-go-round

- It is a text consisting of 3 nested encodings: base64, binary and hex. 
Use Python code to solve this problem :
  ```python
  import base64
  
  def decode_string(s):
      parts = s.split('|')
      decoded_parts = []
      for i in range(len(parts)):
          if i % 3 == 0:  # binary
              decoded_parts.append(chr(int(parts[i], 2)))
          elif i % 3 == 1:  # hexadecimal
              decoded_parts.append(chr(int(parts[i], 16)))
          else:  # base64
              decoded_parts.append(base64.b64decode(parts[i]).decode())
      return ''.join(decoded_parts)
  
  # Đọc từ tệp
  with open('task_file.txt', 'r') as f:
      s = f.read().strip()
  
  print(decode_string(s))

-> flag : CODEBY{bin4ry_h3x_and_b4s3}
