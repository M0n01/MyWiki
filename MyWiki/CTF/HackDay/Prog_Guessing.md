
Un agent du Syndicat a chiffré les communications en utilisant un chiffrement inconnu. Déchiffrez-le et résolvez l'énigme.
Connect to the instance, understand the code and get the file ! `nc challenges.hackday.fr 50395`

#### Codes Python pour faire plein de requêtes et deviner les messages : 

- Pour guess le messages quand on rentre des caractères (ici hello). On peut préciser les lignes qu'on veut afficher :
```python
import subprocess
import time

def execute_command():
	command = "nc challenges.hackday.fr 50395"
	input_text = "hello\n"
	
	# Execute the command and send input
	process = subprocess.Popen(command, shell=True, stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)
	
	process.stdin.write(input_text)
	process.stdin.flush()
	
	# Read and print the first two lines of the output
	output, _ = process.communicate()
	lines = output.split('\n')
	print(lines[0])
	print(lines[1])

# Execute the command 20 times with a 5-second pause in between
for _ in range(100):
	execute_command()
```

- Pour guess le messages quand on rentre un nombre. On peut choisir la ligne et les caractères qu'on veut afficher  :
```python
import subprocess
import time

def execute_command(input_text):
	command = "nc challenges.hackday.fr 50395"
	
	# Execute the command and send input
	process = subprocess.Popen(command, shell=True, stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)
	
	process.stdin.write(input_text + "\n")
	process.stdin.flush()
	
	# Read the output and split into lines
	output, _ = process.communicate()
	lines = output.split('\n')
	
	# Print the last 30 characters of the third line
	if len(lines) >= 3 and len(lines[2]) >= 30:
		print(lines[2][-30:])
	else:
		print(lines[2])

# Test 42, plein de fois (car par brute force on a vu que la réponse est 42)
for i in range(50):
	input_value = str(42)
	execute_command(input_value)
```

Avec toute les messages qu'on a récup lorsque l'on répond "42" on peut deviner le message. Qui est le Flag : HACKDAY{N0T_So_GUESSY_HUH??}

